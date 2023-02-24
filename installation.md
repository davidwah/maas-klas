# Instalasi MAAS JUJU

## Install MAAS
```
sudo snap install maas-test-db
sudo snap install maas --channel=3.2/stable
sudo maas init region+rack --maas-url http://192.168.79.43:5240/MAAS --database-uri maas-test-db:///
sudo maas createadmin --username admin --password surabaya --email admin@example.com
sudo maas apikey --username admin > ~/admin-api-key
```

## Install JUJU
Sebelumnya siapkan VM yang sudah di commissoning dan di beri `tag`
* Instal juju pada vm maas
```
sudo snap install juju --classic
```
* Buat file `maas-cloud.yaml`
```
clouds:
  maas-one:
    type: maas
    auth-types: [oauth1]
    endpoint: http://192.168.79.43:5240/MAAS
```
* Buat file credentials 
```
credentials:
  maas-one:
    anyuser:
      auth-type: oauth1
      maas-oauth: H4Bx:XXXX:XXXX
```
ambil api-key dari profil maas atau hasil generate

* Lalu add-cloud dan add-credential
```
juju add-cloud --client -f maas-cloud.yaml maas-one

juju add-credential --client -f maas-creds.yaml maas-one
```

* Membuat bootstrap
```
juju bootstrap --bootstrap-series=jammy --constraints tags=juju maas-one maas-controller
```

* Membuat model
```
juju models

juju add-model --config default-series=jammy labku

juju switch {pilih-model}
```

* Mengakses JUJU Dashboard
```
juju dashboard

Dashboard 0.8.1 for controller "maas-controller" is enabled at:
  https://192.168.79.37:17070/dashboard
Your login credential is:
  username: admin
  password: paas30863519e
```