# JupyterHub Templates

The original upstream templates can be found here: https://github.com/jupyterhub/jupyterhub/tree/main/share/jupyterhub/templates

## Usage

Mount the right folders in the right places. We're using the `alpine/git` image as an initContainer to clone the repo and move the right folders to the right places, like this in Z2JH config.yaml:
```yaml
hub:
  initContainers:
    - name: git-clone-templates
      image: alpine/git
      args:
        - clone
        - --single-branch
        - --branch=master
        - --depth=1
        - --
        - https://github.com/tqtensor/jupyterhub-templates.git
        - /etc/jupyterhub/custom
      securityContext:
        runAsUser: 0
      volumeMounts:
        - name: custom-templates
          mountPath: /etc/jupyterhub/custom
  extraVolumes:
    - name: custom-templates
      emptyDir: {}
  extraVolumeMounts:
    - name: custom-templates
      mountPath: /etc/jupyterhub/custom
```
