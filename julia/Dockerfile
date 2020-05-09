FROM julia:1.4

WORKDIR /opt/web

{{#files}}
  COPY {{{.}}} {{{.}}}
{{/files}}

RUN julia deps.jl

CMD julia server.jl
