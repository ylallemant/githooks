{{ range .Versions }}
## {{ if and (.Tag.Previous) (not (eq .Tag.Name "Unreleased")) }}[{{ .Tag.Name }}]({{ $.Info.RepositoryURL }}/branchCompare?baseVersion=GT{{ .Tag.Previous.Name }}&targetVersion=GT{{ .Tag.Name }}&_a=files){{ else }}{{ .Tag.Name }}{{ end }} ({{ datetime "2006-01-02" .Tag.Date }})

{{ range .CommitGroups -}}
### {{ .Title }}

{{ range .Commits -}}
* ([{{ .Hash.Short }}]({{ $.Info.RepositoryURL }}/commit/{{ .Hash.Long }})) {{ .Subject }} 
{{ end }}
{{ end -}}

{{- if .RevertCommits -}}
### Reverts

{{ range .RevertCommits -}}
* {{ .Revert.Header }}
{{ end }}
{{ end -}}

{{- if .MergeCommits -}}
### Merges

{{ range .MergeCommits -}}
* ([{{ .Hash.Short }}]({{ $.Info.RepositoryURL }}/pullrequest/{{ .Merge.Ref }}?_a=files)) {{ .Header }}
{{ end }}
{{ end -}}

{{- if .NoteGroups -}}
{{ range .NoteGroups -}}
### {{ .Title }}

{{ range .Notes }}
{{ .Body }}
{{ end }}
{{ end -}}
{{ end -}}
{{ end -}}