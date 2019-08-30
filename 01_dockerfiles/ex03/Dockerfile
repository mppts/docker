FROM ubuntu

RUN apt-get update && \
    apt-get upgrade -y && \
	apt-get install -y ca-certificates openssh-server wget postfix

RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && chmod 777 script.deb.sh && ./script.deb.sh && apt-get install -y gitlab-ce

RUN       sed -i "s|external_url 'http://gitlab.example.com'|external_url 'https://192.168.99.102'|" /etc/gitlab/gitlab.rb && \
           openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 \
           -subj "/C=RU/ST=Denial/L=Moscow/O=Dis/CN=192.168.99.102" \
	       -keyout gitlab.example.com.key  -out gitlab.example.com.crt && \ 
           mkdir -p /etc/gitlab/ssl && chmod 700 /etc/gitlab/ssl && \
           cp gitlab.example.com.key gitlab.example.com.crt /etc/gitlab/ssl/ && \
           sed -i 's|# nginx['redirect_http_to_https_port'] = 80|nginx['redirect_http_to_https_port'] = 80|' /etc/gitlab/gitlab.rb  && \
           echo "192.168.99.102 gitlab.example.com gitlab.example.com" >> /etc/hosts && \
           echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/gitlab.example.com.crt\"" >> /etc/gitlab/gitlab.rb && \
           echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/gitlab.example.com.key\"" >> /etc/gitlab/gitlab.rb && \
           echo "nginx['redirect_http_to_https'] = true" >> /etc/gitlab/gitlab.rb && \
           sed -i "s/# letsencrypt\['enable'\] = nil/letsencrypt\['enable'\] = false/g" /etc/gitlab/gitlab.rb && \
		   sed -i "s/# gitlab_rails['gitlab_shell_ssh_port'] = 22/gitlab_rails['gitlab_shell_ssh_port'] = 22/" /etc/gitlab/gitlab.rb

RUN apt-get update && apt-get install -y tzdata && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

EXPOSE 443 80 22

ENTRYPOINT service ssh start && (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null
#/var/opt/gitlab/nginxa
#/etc/nginx/conf.d/example.conf
#$http_host_with_default
# docker run -it --rm -p 8080:80 -p 8022:22 -p 8443:443 --privileged ex03

# HTTPS
#env GIT_SSL_NO_VERIFY=true git clone https://(docker-machine ip Char):8443/root/<repo>.git

#SSH
# ssh-keygen -t ed25519 -C 'root@gitlab.com' -f id_ed25519
# pbcopy < id_ed25519.pub
# ssh -T -i id_ed25519 -p 8022 git@(docker-machine ip Char)
# env GIT_SSH_COMMAND='ssh -i id_ed25519 -p 8022' git clone git@(docker-machine# ip Char):root/<repo>.git
