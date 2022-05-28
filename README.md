# conversao-peso
Questao 3 do Desafio Docker - Curso KubeDev - Aplicação escrita em C# utilizando ASP.NET Core



# ########################################################################################################################################
# Aplicação escrita em C# utilizando ASP.NET Core

- Endereço do repositório:
<https://github.com/fernandomullerjr/conversao-peso>

- Para criação da imagem Docker foram usadas algumas boas práticas aprendidas no curso até o momento, como:

* Nomear a imagem Docker
* Evitar usar imagens não-oficiais de origem duvidosa
* Sempre especificar a tag nas imagens
* Um processo por Container
* Multistage Build

- A versão final do Dockerfile ficou assim:

~~~Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build-env
WORKDIR /app
COPY . ./
RUN dotnet restore
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "ConversaoPeso.Web.dll"]
~~~


- Observações:
Para o ENTRYPOINT funcionar corretamente, foi necessário nomear o arquivo com extensão dll com o mesmo nome do arquivo com extensão csproj.
Como o arquivo [ConversaoPeso.Web.csproj] está com a versão do NET 5.0 setada de forma fixa, foi necessário usar a versão 5 da imagem do aspnet e do SDK, para que o Container subisse corretamente.

- Comandos utilizados para buildar a imagem e subir o Container:
docker image build -f Dockerfile -t fernandomj90/desafio-docker-questao3-csharp-asp-net:v3 .
docker container run -p 7755:80 -d fernandomj90/desafio-docker-questao3-csharp-asp-net:v3