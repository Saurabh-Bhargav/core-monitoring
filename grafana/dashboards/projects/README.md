Place one folder per project here.

Example:
- `grafana/dashboards/projects/project-alpha/overview.json`
- `grafana/dashboards/projects/project-beta/overview.json`

The Grafana chart is configured to clone this path into the pod and load dashboards from the filesystem.
`foldersFromFilesStructure: true` makes the folder names in Grafana match the folder names in Git.
