### Sobre o Asset
Esta solução oferece conversão de áudios em formato **nmf** para **wav**.
### Arquitetura
Primeiro, o usuário cria um **Cloud Object Storage**(cos) na IBMCloud e passa as informações do bucket no arquivo **config.json**. Em seguida, basta subir os áudios em formato nmf para o cos e executar a aplicação através de requisição.
### Componentes
Este asset é composto por um serviço:
#### 1. nmf-to-wav-converter
- É o arquivo que contém as funções que fazem a conversão de formato de áudio.
### Como realizar o deployment
Para realizar o deployment deste asset, é necessário fazer download dos arquivos disponíveis [neste link](https://portal-de-demos-arquivos.s3.us-south.cloud-object-storage.appdomain.cloud/nmf-to-wav-converter.tar). Também requer a instalação do [docker](https://docs.docker.com/get-docker/).
Depois, siga os seguintes passos:
- Abra o terminal no diretório onde está o arquivo baixado
- Carregue a imagem executando o seguinte comando:
  ```
   docker load -i nmf-to-wav-converter.tar
   ```
- Defina um arquivo **config.json** com as seguintes informações:
  ```
    {
        "COS_ENDPOINT": "",
        "COS_API_KEY_ID": "",
        "COS_SERVICE_CRN": ""
    }
   ```
- Execute a aplicação:
  ```
  docker run -p 3001:3000 -v <absolute/path/to/config.json>:/app/config.json nmf-to-wav-converter
  ```
Após isso, o asset estará disponível em http://localhost:3001
### Guia do usuário
- Faça upload de um ou mais áudios no bucket que você criou. Os áudios convertidos aparecerão no bucket com o mesmo nome dos arquivos que você subiu para o bucket, porém com a terminação **.wav**.
- Para realizar a conversão, faça uma requisição do tipo POST para localhost:3001/processa com um corpo do tipo json:
  ```
  {
    "bucket_input": "<nome do bucket que foram feitos os uploads>",
    "bucket_output": "<nome do bucket para onde serão mandados os áudios convertidos>", 
    "files": [<nome dos arquivos de áudio nmf que estão no bucket_input>]
  }
   ```
O campo **files** é opcional, caso queira realizar a conversão de apenas alguns arquivos nmf que estão no bucket. Caso queira converter todos os arquivos que estão no bucket basta apagar o campo **files** do json. 
