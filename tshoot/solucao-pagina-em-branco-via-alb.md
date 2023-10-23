


## PROBLEMA

- Abrindo URL do Load Balancer, mesmo sintoma, página em branco com titulo "Scaffold":
http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com/








## SOLUÇÃO

- Ajustando app-config e upando nova Docker Image:

DE:

~~~~YAML
app:
  title: Scaffolded Backstage App
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com:3000

organization:
  name: My Company

backend:
  # Used for enabling authentication, secret is shared by all backend plugins
  # See https://backstage.io/docs/auth/service-to-service-auth for
  # information on the format
  # auth:
  #   keys:
  #     - secret: ${BACKEND_SECRET}
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com:7007
  listen:
    port: 7007
    # Uncomment the following host directive to bind to specific interfaces
    host: 0.0.0.0
  csp:
    connect-src: ["'self'", 'http:', 'https:']
~~~~


PARA:

~~~~YAML
app:
  title: Scaffolded Backstage App
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com

organization:
  name: My Company

backend:
  # Used for enabling authentication, secret is shared by all backend plugins
  # See https://backstage.io/docs/auth/service-to-service-auth for
  # information on the format
  # auth:
  #   keys:
  #     - secret: ${BACKEND_SECRET}
  baseUrl: http://k8s-backstag-backstag-c3e6f62e16-1783807425.us-east-1.elb.amazonaws.com
  listen:
    port: 7007
    # Uncomment the following host directive to bind to specific interfaces
    host: 0.0.0.0
  csp:
    connect-src: ["'self'", 'http:', 'https:']
    upgrade-insecure-requests: false # For tutorial purposes only
~~~~


- Foi editado:
      url, removidas as portas
      adicionado o "upgrade-insecure-requests: false"
