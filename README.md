# Helm env file parsing function
Helm helper function to parse .env file and output in yaml format (useful for kubernetes secrets generation)
```
KEY_ENV1=VAL_ENV1      KEY_ENV1: base64(VAL_ENV1)
KEY_ENV2=VAL_ENV2  =>  KEY_ENV2: base64(VAL_ENV2)
KEY_ENV3=VAL_ENV3      KEY_ENV3: base64(VAL_ENV3)
```

## Usage:
In secret template:
```
kind: Secret
metadata:
  name: php-fpm-{{ .Values.environment }}-env
  labels:
    app.kubernetes.io/name: {{ include "helm.fullname" . }}
    helm.sh/chart: {{ include "helm.chart" . }}
    app.kubernetes.io/instance: {{ .Release.Name }}
    app.kubernetes.io/managed-by: {{ .Release.Service }}
    project: {{ include "helm.name" . }}
    environment: {{ .Values.environment }}
type: Opaque
data:
{{ tuple . ".backendenv" | include "env.parseFile" | indent 2 | trimSuffix "\n" }}
```
Then pull env in container:
```
      containers:
      - name: php-fpm
        image: "{{ .Values.app.deployment.images.php.repository }}:{{ .Values.app.deployment.images.php.tag }}"
        imagePullPolicy: {{ .Values.app.deployment.images.php.pullPolicy }}
        envFrom:
          - secretRef:
              name: php-fpm-{{ .Values.environment }}-env
```

## Example file:
```
KEY_ENV1=VAL_ENV1
KEY_ENV2=VAL_ENV2
KEY_ENV3="${VAL_ENV3}"
```

## Output:
```
  KEY_ENV1: base64(VAL_ENV1)
  KEY_ENV2: base64(VAL_ENV2)
  KEY_ENV3: base64("${VAL_ENV3}")
```
