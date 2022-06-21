.PHONY: run build

NOW = $(shell date '+%Y%m%d%H%M%S')

RELEASE_VERSION = v1.0.0

# 当前时间
NOW := $(shell date '+%Y%m%d%H%M%S')

RELEASE_VERSION = v1.0.0

# 应用名称
APP_NAME 		= three_print
# 打包目录
RELEASE_DIR 	= release
# main文件所在目录
MAIN_DIR		= ./

# 不同应用打包在不同目录
RELEASE_APP 	= ${RELEASE_DIR}

all: run

init:
	mkdir -p ./${RELEASE_APP}

build:linux windows darwin

linux:init
	GOOS=linux GOARCH=amd64  go build -ldflags "-w -s" -o ./${RELEASE_APP}/${APP_NAME}-linux-${NOW} ./${MAIN_DIR}/
windows:init
	#GOOS=windows GOARCH=amd64  go build -ldflags "-w -s -H windowsgui" -o ./${RELEASE_APP}/${APP_NAME}-windows-amd64-${NOW}.exe ./${MAIN_DIR}/
	GOOS=windows GOARCH=386  go build -ldflags "-w -s -H windowsgui" -o ./${RELEASE_APP}/${APP_NAME}-windows-386-${NOW}.exe ./${MAIN_DIR}/
darwin:init
	GOOS=darwin GOARCH=amd64 go build -ldflags "-w -s" -o ./${RELEASE_APP}/${APP_NAME}-darwin-amd64-${NOW} ./${MAIN_DIR}/
	GOOS=darwin GOARCH=arm64 go build -ldflags "-w -s" -o ./${RELEASE_APP}/${APP_NAME}-darwin-arm64-${NOW} ./${MAIN_DIR}/

run:
	go run  ./${MAIN_DIR}/main.go

test:
	@go test -v $(shell go list ./...)

clean:
	rm -rf ./${RELEASE_APP}

pack: clean build
	tar -cvf ${APP_NAME}-${NOW}.tar ./${RELEASE_APP}/*
	mv ./${APP_NAME}-${NOW}.tar ./${RELEASE_DIR}
# 二进制压缩，可有可无，只是压缩后二进制小一些（需要在git上下载upx放到环境变量所在的目录下，或者把upx所在目录添加到环境变量）
upx:
	upx --ultra-brute ./$(RELEASE_APP)/*