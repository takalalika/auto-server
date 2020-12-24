# auto-server
  docker-compose ʹ�� nginx-proxy ��һ̨С���²�������Ŀ ʹ���������� ֧��ssl��
  ����Ŀ��Ŀ����Ϊ������ʱ�򷽱�,˳������¼,*����֤���в����������Ҹ߿���*��

 ���䣺

  ����Ŀ�ĳ�����Ϊ�˸��Լ��������¼�¼�����Դ���Ŀ֮������ܻ�ת�䲻������ʹ��docker�

  ��Ŀ��˼�:��˻������Ӹ��ֶ����ݵĹ���ʹ��golangʵ��һ���ű�(���)�ȹ���

## Ŀ¼
   use_ssl��֧��docker-composeһ�����ʹ��ssl

   normal��һЩ�˲�ϲ��https�����������cf���� Let��s Encrypt ����֤�������⡣��������������Ŀ¼�µ�

   tool: һЩ����������,ֱ��ʹ��ip���ʻ���ֱ��ʹ��docker�ܵĶ���

   data: ����Ŀ¼�����ݼ��е���������������ݱ���/ת��

## ����
jwilder/nginx-proxy ����ͨ������д�������Զ�����nginx�������ļ�������Ҫ����ϸ�ڣ�ֻ��Ҫ���������Ϳ���
jrcs/letsencrypt-nginx-proxy-companion ����ͨ��letsencrypt�������https֤�鲢���Զ�������֤
������cf��������������ǿ���һ�δ������Ч������cfҲ����,��������60���᲻���Զ�������֤https�һ�û�в���

## ʹ�ò���
   #### 1.����docker �� docker-compose ������
�������: [docker�ֲ�](https:://yeasy.gitbook.io/docker_practice/install "Markdown")��
   #### 2.����һ�� Docker network
    docker network create nginx-proxy
   #### 3.����ӦĿ¼���� nginx-proxy
    cd auto-server\use_ssl\nginx-proxy
    or
    cd auto-server\normal\nginx
    docker-compose up -d
   #### 4.�޸Ĳ�������Ҫ�����ķ����docker-compose�еĲ���
    ��bitwarden�µ�
      - VIRTUAL_HOST=example.test.com
      - LETSENCRYPT_HOST=example.test.com
      - DEFAULT_EMAIL=xxx@gmail.com
    ����Ӧ�������������޸�,��ʹ��ssl������� ֻ��Ҫ�޸�VIRTUAL_HOST
    һЩ��������������Ҫ�������ã��������ļ��е�ע��

## ��֪������
1.������cloudflare����https�����ϸ�������,��ʹ��use_ssl�еķ������п��ܳ���ssl���鲻�������,ʵ�������ʹ�����ģʽ��������docker-compose��
�ٸĳ��ϸ�ģʽ/ֱ��ʹ��normal��ʽ����cf���ó����ģʽ/cloudflareʹ���ϸ�ģʽ����dns�������㹻�õ��������Ȼ����ֱ��ʹ��use_ssl��������Ҳ��Ϊʲôһ��ʼ��û�з�����������ԭ��

2.file_server ��������������ʾû��Ȩ�� ��Ҫchmod 777 �� app�ļ�����Ȩ

3.wordpress ���ʹ�õ�������װ���ķ�ʽ��װ���⣬docker����(����������������)����ʻ᲻��������Ϊ��������װ�����ݲ�û�д洢�����ص�Ŀ¼�Ŀǰ�Ľ�������ǵ�¼��̨�������°�װһ��

## �����Ų��һЩ��������
������������Ŀ�����в���������ʹ��docker ps -a
��ѯ����Ӧdocker������id ��ʹ�� docker logs -f ����id�ķ�ʽ���Բ�ѯ����������������־�������������jrcs/letsencrypt-nginx-proxy-companionû��ǩ���ɹ�����ϸ����������issue������ֱ��ʹ��normal��ʽ
nginx-proxy�е�������ͨ�� docker volume�����,ĳЩ���������ܻ��뽫��ɾ���������� ��ʹ��docker-compose down  ��ʹ�� docker volume prune ����

## �Ѿ�֧�ֵ���Ŀ
	bitwarden
	chevereto
	gitlab
	file_server(https://files.photo.gallery/demo)
	wordpress
	shadowsoks
	resilio
	syncthing
	transmission

## ����
 #### http://einverne.github.io/post/2017/02/docker-nginx-host-multiple-websites.html
 ������ƪ���¿�ʼ���ڵ�
 ####  https://yeasy.gitbook.io/docker_practice/data_management/volume
 nginx-proxy ��ʹ�õ�docker�����ݾ�û���Զ�����أ���Ҫ�������ݾ�����ѿ��Բ�����ƪ

## ����������Ȥ���ҵĲ���
 #### https://blog.hideo.site
