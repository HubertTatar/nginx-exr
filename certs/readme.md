openssl genrsa -out local-ca.pem 2048
openssl req -x509 -sha256 -new -key local-ca.key -days 360 -subj "/CN=local-ca" -out local-ca.pem

openssl x509 -noout -text -in local-ca.pem
openssl rsa -noout -text -in local-ca.key



openssl genrsa -out client.key 4096
openssl genrsa -out server.key 4096 

openssl rsa -noout -text -in client.key
openssl rsa -noout -text -in server.key


openssl req -new -key client.key -out  client.csr
openssl req -noout -text -in client.csr

openssl req -new -key server.key -out server.csr -config server_ext.cnf
openssl req -noout -text -in server.csr

openssl x509 -req -in client.csr -CA local-ca.pem -CAkey local-ca.key -out client.pem -CAcreateserial -days 360 -sha256  -extfile client_ext.cnf
openssl x509 -req -in server.csr -CA local-ca.pem -CAkey local-ca.key -out server.pem -CAcreateserial -days 360 -sha256  -extensions req_ext -extfile server_ext.cnf

openssl x509 -noout -text -in server.pem
openssl x509 -noout -text -in client.pem



openssl s_client -CAfile local-ca.pem  -connect localhost:443
https://github.com/nabla-c0d3/sslyze
https://testssl.sh/