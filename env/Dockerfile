FROM centos/systemd

MAINTAINER Oleg Snegirev <ol.snegirev@gmail.com>

# Install packages
RUN yum -y install openssh-server openssh-client sudo nano epel-release openssl certmonger; systemctl enable sshd.service

# Enable root and pos accounts
RUN mkdir /var/run/sshd && \
  useradd ansible -s /bin/bash && \
  mkdir -p /home/ansible/.ssh/ && \
  chmod 0700 /home/ansible/.ssh/
RUN echo 'ansible:ansible' | chpasswd



	
# Configure SSHD
RUN chmod -rx /var/run/sshd

RUN sed -ri 's/#PermitRootLogin yes/PermitRootLogin yes/g' /etc/ssh/sshd_config
# http://stackoverflow.com/questions/18173889/cannot-access-centos-sshd-on-docker
RUN sed -ri 's/GSSAPIAuthentication yes/#GSSAPIAuthentication yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/GSSAPICleanupCredentials no/#GSSAPICleanupCredentials no/g' /etc/ssh/sshd_config
RUN sed -ri 's/GSSAPIStrictAcceptorCheck yes/#GSSAPIStrictAcceptorCheck yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/GSSAPIKeyExchange no/#GSSAPIKeyExchange no/g' /etc/ssh/sshd_config
RUN sed -ri 's/GSSAPIEnablek5users no/#GSSAPIEnablek5users no/g' /etc/ssh/sshd_config



RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config

# Deploy ssh keys
RUN mkdir /root/.ssh/
COPY ssh_config /home/ansible/.ssh/config
COPY ansible /home/ansible/.ssh/id_rsa
COPY ansible.pub /home/ansible/.ssh/id_rsa.pub
COPY ansible.pub /home/ansible/.ssh/authorized_keys
COPY copy-ssh-id.rb /home/ansible/copy-ssh-id.rb
COPY ansible-clients /home/ansible/ansible-clients
RUN	chmod 700 /home/ansible/.ssh
RUN chmod 600 /home/ansible/.ssh/authorized_keys
RUN chown -R ansible:ansible /home/ansible/.ssh/

# Configure ansible
RUN bash -c 'echo "ansible ALL=(ALL:ALL) NOPASSWD: ALL" | (EDITOR="tee -a" visudo)'

# Generate sshd keys
RUN ssh-keygen -A





EXPOSE 22

CMD ["/usr/bin/sudo", "/usr/sbin/sshd", "-D", "-o", "ListenAddress=0.0.0.0"]