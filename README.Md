### Usage of Environment Variable
```
1. ymal file for envirment variable 
2. Values used from data/database.db
3. Create database in mongodb
4. create user and password and connect the database
5. copy clusteraddress,username,password
6. goto network address and configure ip for open network e.g. 0.0.0.0

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
