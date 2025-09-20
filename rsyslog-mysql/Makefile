version = latest
arch = amd64
local_rev = 0

build:
	docker build --pull --rm -f "Dockerfile" -t "weslleycsil/rsyslog-mysql:$(arch)-$(version)" . \
		--build-arg VERSION=$(version) \
		--build-arg ARCH=$(arch)

push:
	docker push "weslleycsil/rsyslog-mysql:$(arch)-$(version)"

all:
	docker build --pull --rm -f "Dockerfile" -t "weslleycsil/rsyslog-mysql:$(arch)-$(version)" . \
		--build-arg VERSION=$(version) \
		--build-arg ARCH=$(arch) && \
	docker push "weslleycsil/rsyslog-mysql:$(arch)-$(version)"