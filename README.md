***Meu Diário de Lugares***

Um aplicativo Android nativo, desenvolvido em Kotlin, que permite ao usuário criar um diário pessoal de locais visitados. É possível salvar lugares, adicionar descrições, endereços e manter tudo sincronizado em tempo real com a nuvem, com funcionalidade de cache offline.

Este projeto foi desenvolvido como o projeto final da disciplina de Desenvolvimento de Aplicativos Móveis  do curso de ADS.


**Autores**

* Jaysie Borges de Oliveira RGM: 39225810 - Configuração inicial do projeto, estrutura de dados e criação dos diagramas e do documento inicial do projeto.
* Thauana Vitória Ferreira Farias RGM: 37905236 - Implementação da UI (Compose), ViewModel, Navegação, Sincronização com Firebase e correções de build.

*Nota sobre o Histórico de Commits*

O projeto foi desenvolvido em grande parte localmente antes da integração com o Git. Durante a junção das duas versões do código (a de Jaysie e a de Thauana) para a criação do repositório final, encontramos problemas complexos de mesclagem (merge).

Para salvar o projeto e garantir uma base de código funcional, foi necessário recriar o repositório a partir da versão final do código. Infelizmente, devido a esses problemas técnicos, não foi possível manter o histórico de commits de produção desde o início, o que apagou o histórico de commits da autora inicial (Jaysie Borges).

A seção "Autores" acima reflete a divisão correta do trabalho.


**Funcionalidades Principais**

O aplicativo implementa os seguintes requisitos funcionais:

* RF01: Cadastrar um novo lugar (nome, descrição, endereço).
* RF02: Exibir uma lista com todos os lugares cadastrados pelo usuário.
* RF03: Editar as informações de um lugar já existente.
* RF04: Excluir um lugar da lista.
* RF05: Visualizar os detalhes completos de um lugar em uma tela dedicada.
* RF06: Sincronização em tempo real com o Firebase Firestore, permitindo acesso de múltiplos dispositivos e funcionalidade de cache offline.
* 

**Arquitetura e Tecnologias Utilizadas**

O projeto segue a arquitetura MVVM (Model-View-ViewModel), separando a lógica de UI das regras de negócio.

* Model: Camada de dados (PlaceRepository, PlaceDao), gerenciando as fontes de dados local (Room) e remota (Firebase).

* View: Telas (PlaceListScreen, PlaceDetailScreen, PlaceFormScreen) construídas de forma declarativa com Jetpack Compose.

* ViewModel: (PlaceViewModel) Gerencia o estado da UI, expõe os dados do repositório (via StateFlow) e lida com as ações do usuário.

*Principais Bibliotecas*
* Linguagem: Kotlin (100%)
* UI: Jetpack Compose (para UI declarativa).
* Navegação: Navigation Compose (para gerenciar o fluxo entre as telas).
* Injeção de Dependência: Hilt (Dagger) (para gerenciar o ciclo de vida e injetar dependências).
* Banco de Dados Local: Room (para cache offline).
* Banco de Dados Remoto: Firebase Firestore (para sincronização de dados em tempo real).
* Programação Assíncrona: Kotlin Coroutines (para operações de I/O em background).


**Instruções para Execução**

*1. Importante:* Após clonar, a estrutura de pastas será:

        Projeto final/
        |-- README.md
        |-- MyApplication/  <-- ESTE É O PROJETO ANDROID

Para executar o projeto, você deve abrir apenas a pasta MyApplication/ no Android Studio. Não abra a pasta DiariodeLugares/ de nível superior.

*2. Configurar o Firebase*

O repositório **não** inclui o arquivo de configuração google-services.json. Você deve conectar seu próprio projeto Firebase para que o app possa ser executado.


**Diagrama de navegação**

O fluxo de navegação do aplicativo é gerenciado pelo Navigation Compose e segue uma lógica simples de três telas principais. A navegação começa na lista de lugares (PlaceListScreen), de onde o usuário pode navegar para adicionar um novo local (PlaceFormScreen) ou ver detalhes de um item existente (PlaceDetailScreen). A partir dos detalhes, o usuário pode optar por editar (PlaceFormScreen) ou excluir o local.

*(diagramas visuais se encontram no documento anexado no BlackBoard)*


**Estrutura do Backend (Firebase Firestore)**

Este projeto não utiliza uma API REST tradicional. Ele se comunica diretamente com o Firebase Firestore.

*Coleção Principal: lugares*
* *Caminho:* lugares/{placeId}
* *Estrutura do Documento:*

        {
          "id": "String",
          "nome": "String",
          "descricao": "String",
          "endereco": "String",
          "dataCriacao": "Long"
        }


**Operações de API (CRUD)**

* GET (Leitura): firestore.collection("lugares").addSnapshotListener(...)

Descrição: O app "ouve" a coleção lugares em tempo real. Qualquer mudança no banco de dados é refletida no app instantaneamente e sincronizada com o banco de dados Room local.

* POST (Criação): firestore.collection("lugares").document().set(place)

Descrição: Cria um novo documento na coleção lugares.

Usado em: PlaceFormScreen (modo de cadastro) -> PlaceViewModel.addPlace().

* PUT (Atualização): firestore.collection("lugares").document(place.id).set(place)

Descrição: Atualiza (sobrescreve) um documento existente.

Usado em: PlaceFormScreen (modo de edição) -> PlaceViewModel.updatePlace().

* DELETE (Exclusão): firestore.collection("lugares").document(place.id).delete()

Descrição: Remove um documento da coleção.

Usado em: PlaceDetailScreen -> PlaceViewModel.deletePlace().
