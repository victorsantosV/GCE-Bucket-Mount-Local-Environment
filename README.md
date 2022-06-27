# 05 - Como montar uma Pasta que tenha o bucket como disco

Neste topico, abordaremos na prática, a ideia de usar uma pasta localmente onde o disco seja o BUCKET

<details>
<summary>Instalação</summary>

Para realizar isto precisamos de um compilador de comandos do Google Cloud, chamado GCSFUSE

# 01 - O que é GCSFUSE

```bash
GCSFuse é um pacote que tem como objetivo, 
montar um Storage Bucket ou todos os bucket localmente
```

## Install gcsfuse for install BUCKET in container 

# Linux

If you are running Linux on a 64-bit x86 machine and are happy to install
pre-built binaries (i.e. you don't want to build from source), you need only
ensure fuse is installed, then download and install the latest release package.
The instructions vary by distribution.


## Ubuntu and Debian (latest releases)

The following instructions set up `apt-get` to see updates to gcsfuse, and are
supported for the **focal**, **bionic**, **artful**, **zesty**, **yakkety**, **xenial**,
and **trusty** [releases][ubuntu-releases] of Ubuntu, and the **jessie** and **stretch**
[releases][debian-releases] of Debian. (Run `lsb_release -c` to find your
release codename.) Users of older releases should follow the instructions for
[other distributions](#other-distributions) below.

1.  Add the gcsfuse distribution URL as a package source and import its public
    key:

        export GCSFUSE_REPO=gcsfuse-focal
        echo "deb http://packages.cloud.google.com/apt $GCSFUSE_REPO main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list
        curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

2.  Update the list of packages available and install gcsfuse.

        sudo apt-get update
        sudo apt-get install gcsfuse


</details>

<details>
<summary>Preparando o ambiente</summary>

### Precisamos de algumas coisas antes de montar 

> 1° Precisamos do ID do usuario

> 2° Precisamos de uma pasta 

> 3° Precisamos de um arquivo de usuario em formato json

Seguindo essa ordem:

> 1° ID do usuario

```bash
# para ter o seu id, que será o dono da pasta que será montada o comando é
id -u ${nome_do_user}
```

> 2° Precisamos de uma pasta

```bash
# Criando a pasta
mkdir {nome_da_pasta}

# após a criação, damos permissão para leitura e escrita
chmod 777 ./ {nome_da_pasta}

```

> 3° Arquivo de json

Esta parte é importante pois é nesta parte que daremos acesso ao user para montar a pasta.

>> 1° Precisamos adicionar o email do usuario no IAM e Administrator da GCP

![image_um](./imagens/02.png)

>> 2° Depois de criado precisamos dar permissao para essa conta

entre em contas de serviço >> depois clique sobre a conta criada >> Permissão
![image_um](./imagens/03.png)

>> 3° Agora criar uma chave para este usuario

entraremos em chaves >> criar nova chave >> tipo JSON

![image_um](./imagens/04.png)

Depois de tudo isso feito, será feito o dowload automatico da chave e é apartir disto que faremos o comando para montar a pasta
</details>

<details>
<summary>Comando para execução para montagem</summary>

##### ( ***Para montar devemos nos atentar a alguns parametros para que tudo ocorra como deve ser*** )

```bash

gcsfuse -o allow_other --gid {NUMERO_ID_USER} --uid {NUMERO_ID_USER} --file-mode 777 --dir-mode 777 --key-file={caminho_arquivo_json_permissao} {NOME_BUCKET} /{caminho_pasta_destino_montagem}

```

</details>
