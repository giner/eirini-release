FROM golang:1.12 as builder
WORKDIR /eirini/
COPY src/code.cloudfoundry.org/eirini .
RUN  CGO_ENABLED=0 GOOS=linux go build -mod vendor -a -installsuffix cgo -o eirini ./cmd/opi/
ARG GIT_SHA
RUN if [ -z "$GIT_SHA" ]; then echo "GIT_SHA not set"; exit 1; else : ; fi

FROM scratch
COPY --from=builder /eirini/eirini /workspace/jobs/opi/bin/opi
ENTRYPOINT [ "/workspace/jobs/opi/bin/opi", \
	"connect", \
	"--config", \
	"/workspace/jobs/opi/config/opi.yml" \
]
ARG GIT_SHA
LABEL org.opencontainers.image.revision=$GIT_SHA \
      org.opencontainers.image.source=https://code.cloudfoundry.org/eirini
