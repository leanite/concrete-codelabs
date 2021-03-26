summary: Acessibilidade para deficientes visuais
id: acessibilidade-para-deficientes-visuais
categories: Android
tags: tutorial, codelab
status: Published
authors: Leandro Leite
Feedback Link: http://google.com

# Conceitos de modularização

## Motivação

O que é um módulo?
Problema que resolve: isola as classes, etc
Muito mais fácil fazer import de algum em um package diferente e começar a criar dependências

## Modularização e SOLID

Fazer comparações rápidas entre SOLID e a modularização
Em vez de pensar em classes, pensar em módulos!
o S e o D do SOLID são bem presentes nesse modelo

## Criando um módulo Android

Para criar um módulo novo, devemos ir até **File**, depois selecionar **New** e finalmente **Module**.

![](assets/conceitos-de-modularizacao/new-module.png)

Na nova janela que abriu, selecione a opção **Android Library**.

![](assets/conceitos-de-modularizacao/android-lib-module.png)

Em seguida, dê um nome ao módulo a ser criado e confira se o seu package condiz com o projeto.

![](assets/conceitos-de-modularizacao/create-module.png)

Ao clicar em **Finish** o módulo e seus arquivos serão criados no projeto!

![](assets/conceitos-de-modularizacao/module-created.png)

Alguns arquivos como `.gitignore`, `consumer-rules.pro` e `proguard-rules.pro`, assim como o diretório `libs`, podem ser dispensáveis ao módulo se tiverem as mesmas configurações que o módulo principal do projeto, ou simplesmente não tiver nenhuma necessidade de mudança. Se for o caso, podemos remover esses arquivos sem problemas do nosso novo módulo criado. 

## Estrutura de um módulo

O módulo que criamos possui a estrutura semelhante ao módulo principal **app**.

![](assets/conceitos-de-modularizacao/module-created.png)

Ele possui um arquivo `build.gradle` com as suas configurações de build e um `AndroidManifest.xml` para declarar permissões, Activities, entre outros, exatamente da mesma forma do módulo app.

Especificamente sobre o `build.gradle`, é uma boa prática tornarmos comuns algumas configurações entre os módulos da aplicação. Por exemplo, se cada módulo tiver o seu próprio `compileSdkVersion`, a cada update do SDK Android nós teremos que alterar em todos os arquivos de todos os módulos. Pensando nessa e em outras possíveis alterações, é recomendado configurar o `build.gradle`raiz, o da raíz do projeto que está fora do módulo app, com todas as configurações comuns dos módulos.

**build.gradle raíz**
```
buildscript {
    ...
}

allprojects {
    ...
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

subprojects {
    afterEvaluate { project ->
        if (project.plugins.findPlugin('android') ?: project.plugins.findPlugin('android-library')) {
            android {
                compileSdkVersion 30
                buildToolsVersion "30.0.2"

                defaultConfig {
                    minSdkVersion 21
                    targetSdkVersion 30
                    versionCode 1
                    versionName "1.0"
                    testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
                }

                buildTypes {
                    release {
                        minifyEnabled false
                        proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
                    }
                }
                compileOptions {
                    sourceCompatibility JavaVersion.VERSION_1_8
                    targetCompatibility JavaVersion.VERSION_1_8
                }
                kotlinOptions {
                    jvmTarget = '1.8'
                }
            }
        }
    }
}
```

Basicamente estamos incluindo um bloco que indica que para todos os subprojects, que são os módulos do nosso projeto, configurados com os plugins `android` ou `android-library`, setaremos as configurações especificadas no bloco `android{}`. É importante ressaltar que o módulo **app** também é um subproject.

O `applicationId` configurado deve ser único. Por esse motivo, devemos deixar somente esta configuração no `build.gradle` do módulo principal **app** e removê-la de todos os outros módulos.

**app/build.gradle**
```
plugins {
    id 'com.android.application'
    id 'kotlin-android'
}

android {
    defaultConfig {
        applicationId "com.github.leanite.modules"
    }
}

dependencies {
    ...
}
```

E, com a configuração geral, o nosso módulo recém criado fica com o `build.gradle` bem reduzido, apenas com as suas dependências.

**feature/build.gradle**
```
plugins {
    id 'com.android.library'
    id 'kotlin-android'
}

dependencies {
    ...
}
```

Finalmente, para que o módulo **app** use o módulo **feature** como dependência, basta alterar o `build.gradle` do módulo **app**.

**app/build.gradle**
```
plugins {
    id 'com.android.application'
    id 'kotlin-android'
}

android {
    defaultConfig {
        applicationId "com.github.leanite.modules"
    }
}

dependencies {
    implementation project(':feature')
    ...
}
```

Importante: o arquivo `settings.gradle` possui a configuração de inclusão de todos os módulos do projeto. Sempre devemos conferir se os módulos estão sendo corretamente referenciados nesse arquivo.

**settings.gradle**
```
include ':feature'
include ':app'
rootProject.name = "Modules"
```

## Dependência entre os módulos

Quando temos mais de um módulo no projeto, é comum surgirem partes do código de um módulo que necessitam ser usadas por outros módulos. Devemos avaliar bastante se importar um módulo como dependência de outro é o caminho mais viável, levando em conta a manutenção do código, escalabilidade, preparo para mudanças, entre outros fatores comuns na engenharia de software.

Vamos supor que existam dois módulos, o **módulo A** e o **módulo B**. Se o módulo A necessita utilizar uma classe do módulo B, assim como o módulo B necessita utilizar uma classe do módulo A, é um sinal que devemos repensar a nossa implementação. Situações como essa caracterizam uma **dependência circular** e isso não é permitido. Se importamos o módulo B como uma dependência do módulo A é vice-versa, teremos um erro de compilação acusando dependência circular entre os módulos.

![](assets/conceitos-de-modularizacao/dependencia-circular.png)

O esquema acima ilustra a situação que descrevemos anteriormente: o módulo A possui uma parte de código que interessa ao módulo B e vice-versa. É uma prática muito comum resolver esse impasse extraindo os códigos necessários e colocándo eles em um terceiro módulo comum. 

![](assets/conceitos-de-modularizacao/modulo-base-esquema.png)

Dessa forma, o novo **módulo base** possui o código que interessa aos outros módulos e torna possível o seu uso tanto no módulo A quanto no módulo B. Isso significa que o módulo base pode, sem nenhum problema, ser uma dependência tanto do módulo A quanto do módulo B.

Existe uma variedade de códigos que são comuns aos módulos de um projeto. Geralmente, são os códigos de serviços de API, modelos de negócio, componentes de UI, strings e dimens, extension functions, entre outros. É uma boa prática criar módulos separados para cada tipo de código que precisa ser reutilizado. Quanto mais específico for o módulo, menor a chance de ocorrer uma dependência circular, além de contribuir com a manutenção do código, escalabilidade e saúde do projeto.

## Criando submódulos

Falar de core, base, assets, network, etc

Falar aqui de submódulos e como criá-los :features:cinema



## Comunicação entre módulos

E se quisermos utilizar uma classe de um outro módulo sem extrair esse código para um terceiro módulo?

Interfaces
Injeção de dependência
Ensinar tipo como é feita a modularização da navegação na BrMalls

## Caso real: resolvendo problemas de navegação

Apresentar a solução de navegação baseada no que foi ensinado acima
Duas abordagens: módulo Model comum a 2 módulos que querem se comunicar e módulo Model fechado para cada módulo
    No caso do Model fechado, o módulo de navigation possui um Model de comunicação, como se fosse um modelo de boundary, que faz uma tradução direta com algum modelo a ser usado pelos módulos na comunicação, sem expor seus reais modelos
