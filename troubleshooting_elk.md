from fpdf import FPDF

summary = '''
# ELK Stack on Rancher Desktop Kubernetes: Setup Summary

## 1. Initial Goal
You wanted to run the ELK stack (Elasticsearch, Logstash, Kibana) locally on Rancher Desktop Kubernetes, with a C# app sending logs and traces.

## 2. Deciding on Components
- Determined Logstash was not needed for your use case (C# app sending logs directly).
- Decided to use only Elasticsearch and Kibana.

## 3. Security in Elasticsearch 8.x
- Noted that Elasticsearch 8.x enables security (TLS, authentication) by default.
- Kibana requires an enrollment token to connect securely to Elasticsearch.
- Attempted to generate an enrollment token, but encountered errors due to Elasticsearch not being fully started or not having enough memory.

## 4. Troubleshooting
- Increased memory limits for Elasticsearch to avoid OOM (out-of-memory) errors.
- Waited for Elasticsearch to fully start before generating the enrollment token.
- Encountered HTTPS requirement: Elasticsearch 8.x expects HTTPS by default, leading to connection issues when using HTTP.

## 5. Simplifying for Local Development
- Decided to disable security for local development by setting environment variables:
  - xpack.security.enabled=false
  - xpack.security.http.ssl.enabled=false
- Updated the Kubernetes YAML to reflect these changes.
- Deployed Elasticsearch and Kibana with security disabled, allowing plain HTTP connections and no enrollment tokens or passwords.

## 6. Final Working Setup
- Successfully accessed Kibana and Elasticsearch over HTTP.
- Confirmed that the setup is suitable for local development and testing.

## 7. Key YAML Snippet
Example environment section for Elasticsearch:

    env:
    - name: discovery.type
      value: single-node
    - name: ES_JAVA_OPTS
      value: "-Xms1g -Xmx1g"
    - name: xpack.security.enabled
      value: "false"
    - name: xpack.security.http.ssl.enabled
      value: "false"

## 8. Notes
- For production, always enable security.
- For local dev, disabling security simplifies setup and avoids enrollment tokens.
'''
