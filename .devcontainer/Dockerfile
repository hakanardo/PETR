FROM pytorch/pytorch:2.0.1-cuda11.7-cudnn8-devel as build
RUN pip install vi3o

RUN pip install -U openmim mmsegmentation==0.20.2
RUN mim install mmengine "mmcv>=2.0.0" mmdet==2.24.1
RUN mim install mmdet3d

FROM pytorch/pytorch:2.0.1-cuda11.7-cudnn8-runtime as base
COPY --from=build /opt/conda/lib/python3.10/site-packages/* /opt/conda/lib/python3.10/site-packages/


FROM base as devel
RUN conda install --yes -c conda-forge pylint pyglet fontconfig
RUN apt-get update && apt-get --yes install libx11-6 libgl1 libglu1 fonts-freefont-ttf sudo

ARG USER=user
ARG UID=1000
ARG GID=1000

RUN groupadd -g $GID $USER
RUN useradd -m -u $UID -g $GID -s /bin/bash $USER
RUN echo $USER ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USER && chmod 0440 /etc/sudoers.d/$USER

USER $USER

