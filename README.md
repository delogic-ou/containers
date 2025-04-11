# Delogic Container Library

Various applications, provided by [Delogic](https://delogic.io), containerized and ready to launch. Even though we mostly use these apps for internal purposes, we decided to share them.

## TL;DR

```bash
$ git clone https://github.com/delogic-io/containers.git
$ cd APP/BASE-OS
$ docker build -t delogic-io/APP:TAG .
```

### Prerequisites

- Docker 19.03+

## Build example

You can build the image yourself by cloning the repository, switching directory containing the Dockerfile and executing the `docker build` command.

```bash
$ docker build -t delogic-io/nginx-geoip2:1.24.0-alpine .
```

## License

Copyright &copy; 2023-2025 Delogic OÜ

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License.

You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and limitations under the License.
