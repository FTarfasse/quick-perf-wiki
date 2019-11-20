Please add `https://adoptopenjdk.jfrog.io/adoptopenjdk/jmc-libs` as a repository to your Gradle project:
```
repositories {
	mavenCentral()
	maven { url 'https://adoptopenjdk.jfrog.io/adoptopenjdk/jmc-libs' }
} 
```