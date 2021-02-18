#
# created 2021-02-18, amoreno at g mail
FROM nvidia/cuda:10.2-devel-ubuntu18.04

ARG USER=mcr2
ARG GROUP=mcr2
ARG UID=1000
ARG GID=100
ARG PASSWRD=$USER
ARG GIT_URL=https://github.com/schaltung/mcr2.git

RUN apt-get update \
  && apt-get install git emacs sudo wget -y

RUN apt-get install python3 python3-dev python3-pip -y

RUN cd /usr/bin \
  && ln -s python3 python

RUN python -m pip install certifi==2020.4.5.1 \
  && python -m pip install cycler==0.10.0 \
  && python -m pip install joblib==0.15.1 \
  && python -m pip install kiwisolver==1.2.0 \
  && python -m pip install matplotlib==3.2.1 \
  && python -m pip install numpy==1.18.4 \
  && python -m pip install olefile==0.46 \
  && python -m pip install opencv-python==4.2.0.34 \
  && python -m pip install pandas==1.0.3 \
  && python -m pip install Pillow==7.1.2 \
  && python -m pip install pyparsing==2.4.7 \
  && python -m pip install python-dateutil==2.8.1 \
  && python -m pip install pytz==2020.1 \
  && python -m pip install scikit-learn==0.23.1 \
  && python -m pip install scipy==1.4.1 \
  && python -m pip install six==1.15.0 \
  && python -m pip install threadpoolctl==2.0.0 \
  && python -m pip install torch==1.5.0 \
  && python -m pip install torchvision \
  && python -m pip install tqdm==4.46.0 \
  && python -m pip install progressbar

RUN cd /tmp \
  && wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB \
  && apt-key add GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB \
  && sh -c 'echo deb https://apt.repos.intel.com/mkl all main > /etc/apt/sources.list.d/intel-mkl.list' \
  && apt-get update \
  && apt-get install intel-mkl-64bit-2018.2-046 -y

RUN python -m pip install mkl-fft==1.0.6 \
  && python -m pip install mkl-random==1.0.1.1

RUN mkdir -p /opt/

RUN groupadd -g $GID $GROUP
RUN useradd -m --uid $UID --gid $GID -G $GROUP -s /bin/bash $USER

# Note: become root with: '$ sudo bash' (enter your $PASSWRD)
RUN echo "$USER:$PASSWRD" | chpasswd && adduser $USER sudo


RUN chown -R $USER:$GROUP /opt/ \
  && find /opt/ -type d | xargs chmod g+s

# Because I can't live without this:
RUN echo "alias l='ls -lrt'" >> /etc/profile.d/aliases.sh
RUN echo "alias e='emacs -nw'" >> /etc/profile.d/aliases.sh
RUN echo '\
. /etc/profile ; \
' >> /root/.bashrc

# Set the working directory
USER $USER
WORKDIR /opt/

RUN git clone $GIT_URL

RUN ln -s /mnt/runtime/saved_models /opt/mcr2/

RUN echo '\
. /etc/profile ; \
' >> /home/$USER/.bashrc
