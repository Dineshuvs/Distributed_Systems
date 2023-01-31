docker_create_build_name="dht_create"
docker_join_build_name="dht_join"


sftp:
	docker run --rm -d -p $(p):22 \
	-v ${PWD}/keys/id_rsa.pub:/home/foo/.ssh/keys/id_rsa.pub:ro \
	-v ${PWD}/keys/id_rsa:/etc/ssh/ssh_host_rsa_key:ro \
	atmoz/sftp foo::1001::resources

key_gen:
	ssh-keygen -t rsa -b 4096 -f ${PWD}/keys/id_rsa
	openssl genrsa -out ./keys/encryption-priv.pem
	openssl rsa -in ./keys/encryption-priv.pem -pubout -out ./keys/encryption-pub.pem



build:
	docker build -t $(docker_join_build_name) -f Dockerfile.join .
	docker build -t $(docker_create_build_name) -f Dockerfile.create .

create:
	$(MAKE) sftp p=$(sp)
	docker run -it --rm --publish $(p):$(p) -e ADDRESS=$(a) -e PORT=$(p) -e SSH_PORT=$(sp) $(docker_create_build_name)

join:
	$(MAKE) sftp p=$(sp)
	docker run -it --rm --publish $(p):$(p)  -e ADDRESS=$(a) -e PORT=$(p) -e SSH_PORT=$(sp) -e JOIN_ADDRESS=$(ja) -e JOIN_PORT=$(jp) $(docker_join_build_name)





