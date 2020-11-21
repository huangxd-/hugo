{{ define "main" }}

    {{- range $name, $taxonomy := .Site.Taxonomies.tags -}}
	<a href="/tags/{{ $name | urlize }}">#{{ $name }}<small>({{ .Count }})</small></a>
    {{- end -}}

{{ end }}
