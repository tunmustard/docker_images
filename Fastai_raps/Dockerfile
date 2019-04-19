FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install --no-install-recommends -y apt-utils software-properties-common curl nano unzip openssh-server
RUN apt-get install -y python3 python3-dev python-distribute python3-pip git

# main python packages
RUN pip3 install --upgrade pip
RUN pip3 install --upgrade numpy scipy matplotlib scikit-learn pandas seaborn plotly jupyter statsmodels
RUN pip3 install --upgrade nose tqdm pydot pydotplus watermark geopy joblib

# Jupyter configs
RUN jupyter notebook --allow-root --generate-config -y
RUN echo "c.NotebookApp.password = ''" >> ~/.jupyter/jupyter_notebook_config.py
RUN echo "c.NotebookApp.token = ''" >> ~/.jupyter/jupyter_notebook_config.py
RUN jupyter nbextension enable --py --sys-prefix widgetsnbextension

# TensorFlow 
RUN pip3 install --upgrade tensorflow  

# Keras with TensorFlow backend
RUN pip3 install --upgrade keras

# Graphviz, visualizing trees
RUN apt-get -y install graphviz 


RUN apt-get update

# JDK --fix-missing
RUN apt-get -y  install openjdk-8-jdk
ENV CPLUS_INCLUDE_PATH=/usr/lib/jvm/java-8-openjdk-amd64/include/linux:/usr/lib/jvm/java-1.8.0-openjdk-amd64/include

RUN apt-get -y install cmake 

##Install fast ai
RUN pip3 install fastai



#Clean
RUN apt-get clean && rm -rf /tmp/* /var/tmp/*

	
COPY docker_files/entry-point.sh /

# Final setup: directories, permissions, ssh login, symlinks, etc
RUN mkdir -p /home/user && \
    mkdir -p /var/run/sshd && \
    echo 'root:12345' | chpasswd && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd && \
    chmod a+x /entry-point.sh

WORKDIR /home/user
EXPOSE 22 4545

ENTRYPOINT ["/entry-point.sh"]
CMD ["shell"]

