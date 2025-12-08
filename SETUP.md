# Setup Instructions

## ✅ Files Created

The following files have been added to your `Java-assignment` folder:

1. **doc.py** - Azure OpenAI Agent caller script (customized for servlet app)
2. **requirements.txt** - Python dependencies for doc.py
3. **.github/workflows/main.yaml** - GitHub Actions workflow
4. **.gitignore** - Ignore build artifacts and test files
5. **README.md** - Project documentation

## 🚀 Setup Steps

### 1. Create GitHub Repository

```bash
cd C:\Users\PAShraavya\aihub\Java-assignment

# Create a new repository on GitHub first at:
# https://github.com/new
# Repository name: java-assignment (or your preferred name)

# Then link it:
git remote add origin https://github.com/Shraavyapa/java-assignment.git
git branch -M main
git push -u origin main
```

### 2. Configure GitHub Secrets

Go to your repository settings and add these secrets (use the SAME values from First-project):

**Settings → Secrets and variables → Actions → New repository secret**

Add these 5 secrets:

1. **ACR_NAME**: Your Azure Container Registry name
2. **AZURE_CREDENTIALS**: Service principal JSON (same as First-project)
3. **AZURE_AIF_ENDPOINT**: `https://deevelop.services.ai.azure.com/openai`
4. **AZURE_AIF_AGENT_ID**: `asst_7YIWBqHMmACnG0MR3NRJZgmk`
5. **AZURE_AIF_API_KEY**: Your API key (optional, uses Entra token by default)

### 3. Trigger the Workflow

```bash
# Make any change (e.g., update README)
echo "" >> README.md
git add .
git commit -m "Trigger workflow"
git push origin main
```

The workflow will:
1. ✅ Call Azure OpenAI Agent (GPT-4.1)
2. ✅ Generate optimized Dockerfile for Tomcat servlet app
3. ✅ Build multi-stage Docker image
4. ✅ Push to ACR as `legacy-servlet-app:latest`

### 4. Monitor Progress

- **GitHub Actions**: https://github.com/Shraavyapa/java-assignment/actions
- **ACR**: Azure Portal → Your Container Registry → Repositories → `legacy-servlet-app`

## 📁 File Structure

```
Java-assignment/
├── .github/workflows/main.yaml    ← GitHub Actions workflow
├── .gitignore                     ← Ignore files
├── README.md                      ← Documentation
├── doc.py                         ← AI agent caller
├── requirements.txt               ← Python deps
├── pom.xml                        ← Maven config
└── src/
    └── main/
        ├── java/com/kyndryl/
        │   └── HelloLegacyServlet.java
        └── webapp/WEB-INF/
            └── web.xml
```

## ✅ What's Different from First-project?

- **Image name**: `legacy-servlet-app` (instead of `simple-fastapi`)
- **Agent prompt**: Customized for Tomcat servlet app with WAR packaging
- **Dockerfile**: Will use Maven + Tomcat (multi-stage build)
- **Same secrets**: Reuses all Azure resources from First-project

## 🎯 Expected Dockerfile Output

The AI agent will generate something like:

```dockerfile
# Build stage
FROM maven:3.8-jdk-8 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package

# Runtime stage
FROM tomcat:9-jre8-alpine
COPY --from=build /app/target/simple-legacy-app.war /usr/local/tomcat/webapps/
EXPOSE 8080
CMD ["catalina.sh", "run"]
```

## 🧪 Testing Locally (Optional)

```bash
# After workflow runs and Dockerfile is generated:
docker pull <your-acr>.azurecr.io/legacy-servlet-app:latest
docker run -p 8080:8080 <your-acr>.azurecr.io/legacy-servlet-app:latest

# Test the app
curl http://localhost:8080/simple-legacy-app/hello
```

---

**Ready to deploy!** 🚀

Just create the GitHub repo, add secrets, and push!
