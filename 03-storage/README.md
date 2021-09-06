# Docker Storage

## Storage Bind

Bind irá mapear uma pasta do seu host para o container

Vamos criar uma pasta no host e mapea-la para o container:

```bash
mkdir /html
```

Vamos mapear essa pasta criada:

```bash
docker run -d --name container-volume -p 80:80 -v /html:/usr/share/nginx/html nginx:latest
```

## Storage Tmps

Cria um volume para armazenar aquivos temporários, evitando armazenar dados na camada de escrita

```bash
docker run -d --name tmptest --mount type=tmpfs,destination=/cache,tmpfs-size=1000000 busybox sleep 3600
```

Comando dd para criar o arquivo de 1mb

```bash
dd if=/dev/zero of=output.file bs=1024 count=1024
```

## Storage Volume

Mount é mais verboso do que utilizar -v ou --volume

Criando um disco utilizando -v

### meuPrimeiroVolume:/path/inContainer:ro

```bash
docker volume create meuPrimeiroVolume
```

### listar volume

```bash
docker volume list
```

### inspecionar o volume criado

```bash
docker volume inspect meuPrimeiroVolume
```

### remover o volume

```bash
docker volume rm meuPrimeiroVolume
```

### Criado container com volume

```bash
docker run -d --name container-volume -p 80:80 --mount source=meuPrimeiroVolume,target=/usr/share/nginx/ nginx:latest
```

### Inspecione o container criado

```bash
docker inspect container-volume
```
