# Задача 1
    mkdir database_files
    mkdir database_backup
    
    docker run -d --name postgres \
    -e POSTGRES_PASSWORD=admin \
    -v $(pwd)/database_files:/var/lib/postgresql/data \
    -v $(pwd)/database_backup:/etc/postgresql \
    postgres
    
# Задача 2
<img width="825" alt="Screenshot 2023-03-03 at 13 53 25" src="https://user-images.githubusercontent.com/97674120/222642317-963fc7f5-9ab3-46b1-9613-0c5d494c8578.png">
<img width="714" alt="Screenshot 2023-03-03 at 14 08 42" src="https://user-images.githubusercontent.com/97674120/222644519-a3c16269-6a37-4848-ac8f-1a8b39f2a98e.png">
<img width="695" alt="Screenshot 2023-03-03 at 14 09 31" src="https://user-images.githubusercontent.com/97674120/222644593-fd75d457-93fe-4622-94a6-fb5fbb49fff9.png">
<img width="527" alt="Screenshot 2023-03-03 at 14 19 19" src="https://user-images.githubusercontent.com/97674120/222646153-46027e1f-b91c-48c4-b24d-cb74c6e839df.png">

# Задача 3
