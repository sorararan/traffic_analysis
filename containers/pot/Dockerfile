FROM centos:6

RUN yum -y update && yum clean all

RUN yum reinstall -y glibc-common && yum clean all

RUN yum install -y vim wget gcc iputils tcp_wrappers libcap-devel openssl-devel tcp_wrappers-devel xinetd nc wireshark tcpdump ftp && yum clean all

COPY src/builddefs.h src/vsftpd.chroot_list src/vsftpd src/vsftpd.conf /root/

RUN wget https://ftp.icm.edu.pl/packages/vsftpd/vsftpd-2.3.4.tar.gz -P /usr/local/src
RUN tar xfvz /usr/local/src/vsftpd-2.3.4.tar.gz -C /usr/local/src
RUN mv /root/builddefs.h /usr/local/src/vsftpd-2.3.4/
WORKDIR /usr/local/src/vsftpd-2.3.4/
RUN make clean
RUN make
RUN make install
RUN mkdir /etc/vsftpd
RUN cp /usr/local/src/vsftpd-2.3.4/vsftpd.conf /etc/vsftpd/
RUN mv /root/vsftpd.chroot_list /etc/vsftpd/
RUN cp /usr/local/src/vsftpd-2.3.4/RedHat/vsftpd.pam /etc/pam.d/vsftpd
RUN mv /root/vsftpd /etc/xinetd.d/
RUN mv /root/vsftpd.conf /etc/vsftpd/
RUN /etc/init.d/xinetd start

CMD ["bash"]

