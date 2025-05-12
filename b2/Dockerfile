FROM phusion/baseimage:jammy-1.0.3

RUN rm -f /etc/service/sshd/down
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config

RUN sed -i "s/http:\/\/archive.ubuntu.com/http:\/\/mirrors.aliyun.com/g" /etc/apt/sources.list
RUN apt update
RUN apt install nginx -y
RUN apt install python3 -y

RUN curl https://bootstrap.pypa.io/get-pip.py -o /tmp/get-pip.py
RUN python3 /tmp/get-pip.py
# python3.8
RUN pip3 install "jinja2<3.1"
RUN pip3 install "werkzeug==0.16.1"
RUN pip3 install "itsdangerous==1.1.0"
RUN pip3 install "SQLAlchemy==1.3.24"
RUN pip3 install "email_validator==1.1.3"
RUN pip3 install "Flask==1.0.3"
RUN pip3 install "flask_sqlalchemy==2.4.0"
RUN pip3 install "flask_bcrypt==0.7.1"
RUN pip3 install "flask_login==0.4.1"
RUN pip3 install "flask_mail==0.9.1"
RUN pip3 install "flask_wtf==0.14.2"
RUN pip3 install "pyyaml==3.12"

ADD ./default /etc/nginx/sites-enabled/

RUN groupadd glzjin && \
    useradd -g glzjin glzjin -m && \
    password=$(openssl passwd -1 -salt 'abcdefg' '123456') && \
    sed -i 's/^glzjin:!/glzjin:'$password'/g' /etc/shadow

RUN mkdir /home/glzjin/Flaskshop
COPY ./Flaskshop/ /home/glzjin/Flaskshop/
RUN chown -R glzjin:glzjin /home/glzjin/Flaskshop/

RUN echo 'flag{glzjin_wants_a_girl_friend}' > /flag.txt

ADD ./start.sh /etc/my_init.d/
RUN chmod u+x /etc/my_init.d/start.sh
