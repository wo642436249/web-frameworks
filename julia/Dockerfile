FROM julia:1.4

WORKDIR /opt/web

{{#sources}}
  COPY {{{.}}} {{{.}}}
{{/sources}}

RUN julia deps.jl

CMD julia server.jl
