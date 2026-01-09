---
{{- with .Title }}title: {{ . | safeHTML }}{{ end }}
{{- with .Date }}
date: {{ .Format "2006-01-02T15:04:05Z07:00" }}{{ end }}
{{- with .Params.subtitle }}
subtitle: {{ . | safeHTML }}{{ end }}
{{- with .Params.categories }}
categories: {{ . }}{{ end }}
{{- with .Params.tags }}
tags: {{ . }}{{ end }}
{{- if isset .Params "toc" }}
toc: {{ .Params.toc }}{{ end }}
{{- if .Draft }}
draft: true{{ end }}
---
{{ .RawContent }}