name: Java CI with Maven

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        java-version: [11, 17]

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v3

    - name: Set up JDK
      uses: actions/setup-java@v3
      with:
        distribution: 'temurin'
        java-version: ${{ matrix.java-version }}

    - name: Cache Maven dependencies
      uses: actions/cache@v3
      with:
        path: ~/.m2
        key: maven-${{ runner.os }}-${{ hashFiles('**/pom.xml') }}
        restore-keys: |
          maven-${{ runner.os }}-

    - name: Build with Maven
      run: mvn clean install

    - name: Run Tests
      run: mvn test
