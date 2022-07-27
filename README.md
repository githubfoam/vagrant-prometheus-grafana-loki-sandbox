# vagrant-prometheus-grafana-loki-sandbox
~~~~
vagrant up vg-pro-gr-lk-02
vagrant ssh vg-pro-gr-lk-02

vagrant@vg-pro-gr-lk-02:~$ docker-compose -f /vagrant/docker-compose-latest.yml up &

http://192.168.53.9:9090
http://192.168.53.9:3000 admin/test

vagrant@vg-pro-gr-lk-02:~$ docker-compose -f /vagrant/docker-compose-latest.yml stop
vagrant@vg-pro-gr-lk-02:~$ docker-compose -f /vagrant/docker-compose-latest.yml down
vagrant@vg-pro-gr-lk-02:~$ docker-compose -f /vagrant/docker-compose-latest.yml ps
~~~~