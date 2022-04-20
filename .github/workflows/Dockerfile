FROM node:14

#Install OracleDb Instant Library
RUN apt update && apt install -y libaio1 libc6 wget 
RUN mkdir -p /opt/oracle
ADD /lib/instantclient-basic-linuxx64.zip /opt/oracle
RUN unzip /opt/oracle/instantclient-basic-linuxx64.zip -d /opt/oracle
RUN sh -c "echo /opt/oracle/instantclient_21_4 > /etc/ld.so.conf.d/oracle-instantclient.conf"
RUN chmod 777 /opt/oracle/instantclient_21_4/network/admin/
COPY /TNS/tnsnames.ora  /opt/oracle/instantclient_21_4/network/admin/
RUN ldconfig
# Create Working Directory

WORKDIR /app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .
ENV PORT 3002
ENV NODE_ENV "development"
ENV Password "Portal123"
ENV ConnectString "172.24.47.160:1521/payweb05"
ENV VaccineDetails "EMPLOYEE_VACCINATION_DATA"
ENV TOKEN_EXCHANGE_CLIENT_ID "9ce43e17-d4de-422d-b053-39aa007fe609"
ENV TOKEN_EXCHANGE_CLIENT_SECRET "Bu17Q~Df4nRahGzcEsjqVS21QfQBrcnXmI8E8"
# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
       && apt-get install -y --no-install-recommends dialog \
       && apt-get update \
       && apt-get install -y --no-install-recommends openssh-server \
       && echo "$SSH_PASSWD" | chpasswd

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/

RUN chmod u+x /usr/local/bin/init.sh

EXPOSE 2222
EXPOSE 1521
EXPOSE 5001
EXPOSE 5002
EXPOSE 3002
ENTRYPOINT ["init.sh"]