Setting Up a Maven Project with GitHub and GitHub Actions
This guide walks you through:

Setting up a Maven project locally.
Initializing it as a Git project.
Testing Maven commands.
Pushing it to GitHub.
Using GitHub Actions to build a .jar file and upload it to the repo.
✅ Prerequisites
Java (JDK 8 or later)
Maven installed (mvn -v)
Git installed
GitHub account
⚡ Step 1: Create a Maven Project Locally
mvn archetype:generate -DgroupId=com.example.app -DartifactId=myapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
Navigate into the project:

cd myapp
⚖ Step 2: Initialize Git Repo
git init
git add .
git commit -m "Initial commit"
Create a GitHub repo (manually or via CLI), then add it as remote:

git remote add origin https://github.com/YOUR_USERNAME/myapp.git
git branch -M main
git push -u origin main
🔧 Step 3: Test Maven Commands
Build the project:

mvn clean package
Check the generated JAR file:

ls target/*.jar
Run the JAR file (if it has a main method):

java -cp target/myapp-1.0-SNAPSHOT.jar com.example.app.App
⚙️ Step 4: Set Up GitHub Actions
Create directory and file:

mkdir -p .github/workflows
touch .github/workflows/maven-build.yml
maven-build.yml Content
name: Build and Package JAR

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up JDK
        uses: actions/setup-java@v3
        with:
          distribution: 'temurin'
          java-version: '17'

      - name: Build with Maven
        run: mvn clean package

      - name: Upload artifact to repo
        uses: actions/upload-artifact@v4
        with:
          name: myapp-jar
          path: target/myapp-1.0-SNAPSHOT.jar
🎈 Step 5: Access the JAR from GitHub Actions
After a push to main, go to Actions tab.
Select the workflow run.
Under Artifacts, download myapp-jar.
🚀 Final Tips
You can modify pom.xml for more dependencies or plugins.
Use mvn test to run unit tests.
Store secrets (like deploy credentials) in GitHub secrets.
Happy Building! ✨
