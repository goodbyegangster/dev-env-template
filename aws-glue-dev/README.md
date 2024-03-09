# aws-glue-dev

## requirements

### aws profile

dev container で利用される IAM ユーザーを、`aws-glue-dev` というプロファイル名で設定する

### file permission

```sh
$ sudo chmod 666 ~/.aws/config ~/.aws/credentials
$ sudo chmod -R 777 ./workspace
```

コンテナ上では、以下のユーザーが利用されるため、ローカル側ファイル・パーミッションを変更しておく

```sh
[glue_user@08a66ad59d8d workspace]$ id
uid=10000(glue_user) gid=0(root) groups=0(root)
[glue_user@08a66ad59d8d workspace]$ umask
0022
```

## VS Code Dev Containers (Recommend)

- Dev Containers
  - Open Folder in Container
    - Project Root (dev-env-template.code-workspace)

### in Dev Container

REPL (Read-Evaluate-Print Loop)

```sh
[glue_user@f4be0b8ca9e6 workspace]$ pyspark
```

job

```sh
[glue_user@f4be0b8ca9e6 workspace]$ spark-submit /path/to/file.py
```

## docker compose

```sh
$ docker compose up
```

## docker run

```sh
$ export PROFILE_NAME=aws-glue-dev
$ export WORKSPACE_LOCATION=./workspace
$ docker run -it \
  -v ~/.aws:/home/glue_user/.aws \
  -v $WORKSPACE_LOCATION:/home/glue_user/workspace/ \
  -e AWS_PROFILE=aws-glue-dev \
  -e DISABLE_SSL=true \
  -e DATALAKE_FORMATS=hudi,delta,iceberg \
  --rm \
  -p 4040:4040 -p 8888:8888 -p 8998:8998 -p 18080:18080 \
  --entrypoint bash \
  amazon/aws-glue-libs:glue_libs_4.0.0_image_01
```

## references

- [Developing using a Docker image](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-libraries.html#develop-local-docker-image)
- [ローカル開発の制限](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-libraries.html#local-dev-restrictions)
- [Docker Hub: aws-glue-libs](https://hub.docker.com/r/amazon/aws-glue-libs)
