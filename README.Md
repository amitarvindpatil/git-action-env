### Usage of Environment Variable
```
1. ymal file for envirment variable 
2. Values used from data/database.db
3. Create database in mongodb
4. create user and password and connect the database
5. copy clusteraddress,username,password
6. goto network address and configure ip for open network e.g. 0.0.0.0

Note - Example of Default Env Variable (https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-variables#default-environment-variables )


name: Deployment
on:
  push:
    branches:
      - master
      - dev
env:
  MONGODB_DB_NAME:
jobs:
  test:
    env:
      # Cluster-demo
      MONGODB_CLUSTER_ADDRESS: cluster-demo.5kx34o1.mongodb.net
      MONGODB_USERNAME: amitpatil04041993
      MONGODB_PASSWORD: LI8b3qtpMVUk3kSC
      PORT: 8080
    runs-on: ubuntu-latest
    steps:
      - name: Get Code
        uses: actions/checkout@v3
      - name: Cache dependencies
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: npm-deps-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        run: npm ci
      - name: Run server
        run: npm start & npx wait-on http://127.0.0.1:$PORT
      - name: Run tests
        run: npm test
      - name: Output information
        run: |
          echo "MONGODB_USERNAME: ${{ env.MONGODB_USERNAME }}"
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Output information
        run: |        
          echo "MONGODB_USERNAME: ${{ env.MONGODB_USERNAME }}"
          echo "MONGODB_DB_NAME: $MONGODB_DB_NAME"


### Usage of Secrets (for credentials )
```
1. Goto repo settings --> Secrets --> Actions
2. Set Repository Secrets

jobs:
  test:
    env:
      
      MONGODB_CLUSTER_ADDRESS: cluster-demo.5kx34o1.mongodb.net
<!-- Secrets Declaration -->
      MONGODB_USERNAME: ${{ secrets.MONGODB_USERNAME }}    
      MONGODB_PASSWORD: ${{ secrets.MONGODB_PASSWORD }}

### Usage of Environment Secrets (for credentials )
```
1. Goto repo settings --> Environment --> config enviroments
2. goto Enviroment Secrets panel and add MONGODB_USERNAME and MONGODB_PASSWORD
3. it can also allow to specify the deployment branch such as master,
  
env:
  MONGODB_DB_NAME:
jobs:
  test:
  <!-- Enviroment Declaration -->
    environment: testing
    env:
      # Cluster-demo
      # LI8b3qtpMVUk3kSC
      MONGODB_CLUSTER_ADDRESS: cluster-demo.5kx34o1.mongodb.net
      MONGODB_USERNAME: ${{ secrets.MONGODB_USERNAME }}
      MONGODB_PASSWORD: ${{ secrets.MONGODB_PASSWORD }}
      PORT: 8080