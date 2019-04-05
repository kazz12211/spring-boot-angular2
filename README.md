# spring-boot-angular2

This project is a template which integrates Angular 2 frontend into Spring Boot project.

このプロジェクトはSpring BootプロジェクトにAngular 2アプリケーションを統合する雛形です。


## Create Spring Boot project - Spring Bootプロジェクトの生成

Use Spring Initializr to create Spring Boot project. The result is Spring Boot project folder (spring-boot-angular2 in this case).

Spring Initializrを使ってSpring Bootプロジェクトを生成します。結果としてSpring Bootプロジェクトフォルダーが作られます。（この場合はspring-boot-angular2です）

## Create Angular 2 project - Angular 2プロジェクトの生成

Inside spring-boot-angular2 folder, change directory to src/main/resources, then exec `ng new frontend` to create Angular project.

spring-boot-angular2フォルダーの中で、src/main/resourceにワーキングディレクトリを移動し、`ng new frontend'を実行してAngularプロジェクトを生成します。

## Project directory structure - ディレクトリ構成

```
spring-boot-angular2/
    src/
        main/
            java/
                jp.tsubakicraft/demo/
                    DemoApplication.java
            resources/
                frontend/ (Angular 2 app)
                    e2e/
                    node_modules/
                    src/
                    angular.json
                    package.json
                    tsconfig.json
                    ....
                static/ (Spring Boot web resources)
                templates/
                application.properties
        test/
            ....
    pom.xml
    ....
```


## Modify angular.json - angular.jsonの編集

Edit angular.json in frontend project, change architect.build.options.outputPath to "../static" where Spring Boot web resources reside.

frontendプロジェクト内のangular.jsonを編集し、architect.build.options.outputPathを"../static"に変更します。このパスにはSpring Bootのwebリソースが置かれます。

```
      "architect": {
        "build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "../static",
            "index": "src/index.html",
            "main": "src/main.ts",
            "polyfills": "src/polyfills.ts",
            "tsConfig": "src/tsconfig.app.json",
            "assets": [
              "src/favicon.ico",
              "src/assets"
            ],
            "styles": [
              "src/styles.css"
            ],
            "scripts": [],
            "es5BrowserSupport": true
          },
```

## Modify pom.xml - pom.xmlの編集

Add plugin to make maven build frontend.

pluginを追加して、mavenがビルドを行う際にfrontendをビルドします。

```
	<build>
		<plugins>
			<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>exec-maven-plugin</artifactId>
				<version>1.6.0</version>
				<executions>
					<execution>
						<phase>validate</phase>
						<goals>
							<goal>exec</goal>
						</goals>
					</execution>
				</executions>
				<configuration>
					<executable>ng</executable>
					<workingDirectory>src/main/resources/frontend</workingDirectory>
					<arguments>
						<argument>build</argument>
					</arguments>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
```
