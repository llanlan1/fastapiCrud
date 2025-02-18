# FastAPI CRUD API
A simple CRUD API using FastAPI and PostgreSQL.

## Features
? Create, Read items  
? Uses FastAPI for high performance  
? PostgreSQL as database  
? Docker-ready  

## Setup
1. Install dependencies:
   ```powershell
   pip install -r requirements.txt

##Run PostgreSQL (Docker):

   ```Powershell
   docker run --name postgres-db -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -e POSTGRES_DB=items_db -p 5432:5432 -d postgres

##Run migrations:
   ```powershell
   alembic upgrade head

##Start API:
   ```powershell
   uvicorn app.main:app --reload
   
   