# Copyright 2019 HTCondor Team, Computer Sciences Department,
# University of Wisconsin-Madison, WI.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM jupyter/minimal-notebook:7d427e7a4dde
ARG HTCONDOR_VERSION=8.8

USER root

RUN apt-get update \
 && apt-get -y install --no-install-recommends gnupg git  \
 && wget -qO - https://research.cs.wisc.edu/htcondor/ubuntu/HTCondor-Release.gpg.key | apt-key add - \
 && echo "deb  http://research.cs.wisc.edu/htcondor/ubuntu/${HTCONDOR_VERSION}/bionic bionic contrib" >> /etc/apt/sources.list \
 && apt-get -y update \
 && apt-get -y install htcondor \
 && apt-get -y clean \
 && rm -rf /var/lib/apt/lists/*

COPY binder/condor_config.local /etc/condor/condor_config.local
COPY binder/entrypoint.sh ${HOME}/.entrypoint.sh
COPY /tutorials ${HOME}/tutorials
RUN pip install --no-cache-dir htcondor==${HTCONDOR_VERSION}.* nbstripout \
 && nbstripout `find ${HOME}/tutorials -name '*.ipynb'` \
 && chmod +x ${HOME}/.entrypoint.sh \
 && chown -R ${NB_UID} ${HOME}

USER ${NB_USER}
WORKDIR ${HOME}/tutorials
ENTRYPOINT ["/home/jovyan/.entrypoint.sh"]
CMD ["jupyter", "lab"]
