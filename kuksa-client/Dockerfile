# Copyright Robert Bosch GmbH, 2020. Part of the Eclipse Kuksa Project.
#
# All rights reserved. This configuration file is provided to you under the
# terms and conditions of the Eclipse Distribution License v1.0 which
# accompanies this distribution, and is available at
# http://www.eclipse.org/org/documents/edl-v10.php
#

# Note: This dockerfile needs to be executed one level above in the root folder

FROM python:3.10-alpine as build
RUN apk update && apk add git alpine-sdk linux-headers
COPY kuksa-client /kuksa.val/kuksa-client/
COPY kuksa_certificates /kuksa.val/kuksa_certificates/
COPY proto/kuksa/val/v1/*.proto /kuksa.val/kuksa-client/kuksa/val/v1/
WORKDIR /kuksa.val/kuksa-client
RUN pip install --upgrade pip build
RUN rm -rf dist
RUN python3 -m build
RUN mkdir /kuksa-client
RUN pip install --target /kuksa-client --no-cache-dir dist/*.whl

FROM python:3.10-alpine

RUN apk add --no-cache libstdc++
COPY --from=build /kuksa-client /kuksa-client
ENV PYTHONUNBUFFERED=yes
ENV GRPC_ENABLE_FORK_SUPPORT=false
ENV PYTHONPATH=/kuksa-client
WORKDIR /kuksa-client
ENTRYPOINT ["bin/kuksa-client"]
