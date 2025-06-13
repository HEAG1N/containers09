Optimizarea imaginilor

Scopul lucrării

Scopul lucrării este de a se familiariza cu metodele de optimizare a imaginilor.

Sarcina

Compararea diferitelor metode de optimizare a imaginilor:

Ștergerea fișierelor temporare și a dependențelor neutilizate

Reducerea numărului de straturi

Utilizarea unei imagini de bază minime

Reasambalarea imaginii

Utilizarea tuturor metodelor

Execuție

Cream un repozitoriu containers09 și il copiam pe computer. În directorul containers09 cream directorul site și plasam în el fișierele site-ului (html, css, js).

![433029256-618138f3-7b0b-488c-b728-1b39b27b0946](https://github.com/user-attachments/assets/04970a7f-3e6e-40e7-937f-93ec0c4dc256)

Pentru teste de optimizare a imaginilor va fi utilizată imaginea creată în bază Dockerfile.raw:

![433029456-3a66dc55-6750-48f2-b96e-f4f81674dd3b](https://github.com/user-attachments/assets/a0f6e2cf-8b01-43f6-994e-816dd0bc5504)

Cream în folderul containers09 și construim imaginea cu numele mynginx:raw:

![433029594-c88db3a8-f3ca-4f3b-bb45-a1dd62117240](https://github.com/user-attachments/assets/0c30dda3-9cbd-4687-9ed5-3c02e0a3ba86)

Eliminarea dependențelor neutilizate și a fișierelor temporare

Eliminam fișierele temporare și dependențele neutilizate în Dockerfile.clean:

![433029866-4e63e86e-cc9c-474c-9b17-831a48f35389](https://github.com/user-attachments/assets/2db4443a-907a-47ab-9779-eee312cd731a)

Asamblam imaginea cu numele mynginx:clean și verificam dimensiunea:

![433029939-02d6ba01-af57-43dd-a304-c4e7e7fcb9cc](https://github.com/user-attachments/assets/0b6c9e05-bff7-4f86-99d9-8ed85f433b80)

![433029968-a0e3129e-cc33-4c2e-a06b-8cf163ef5973](https://github.com/user-attachments/assets/a2406964-755b-4810-b1fd-81b91db0ce9d)

Minimizarea numărului de straturi

Minimizam numărul de straturi în Dockerfile.few:

![433030086-4b91abdc-f23c-413a-a092-9f15e08574a2](https://github.com/user-attachments/assets/7e71d448-de5b-45b2-a186-3398c559fa47)

Construim imaginea cu numele mynginx:few și verificam dimensiunea:

![433030148-cbcbe556-65f4-43b4-9195-9e0109c864e5](https://github.com/user-attachments/assets/47a43c9c-236d-48bc-9cbc-4aa03a7c1c74)

![433030180-a3c865f5-c1f7-439d-990e-258987b4104c](https://github.com/user-attachments/assets/11f8f5ed-2dc0-4ced-b239-4904d8940273)

Utilizarea unei imagini de bază minime

Înlocuim imaginea de bază cu alpine în Dockerfile.alpine:

![433030289-0a01b525-c993-4d14-9255-62f15e208bf3](https://github.com/user-attachments/assets/77207a9d-de7d-40b3-8918-17092de2a378)

Construim imaginea cu numele mynginx:alpine și verificam dimensiunea:

![433030389-58c64778-b3c7-49aa-81b7-fe64e3d35bb9](https://github.com/user-attachments/assets/2ff5aeaf-a8b4-40c0-9eb7-4ae9c456a661)

![433030408-1c11b129-7e40-46e4-80cf-76a72e37efa2](https://github.com/user-attachments/assets/411e1db5-d7bc-4deb-afe5-5ebfadcbf3de)

Repachetarea imaginii

Repachetam imaginea mynginx:raw în mynginx:repack:
```
docker container create --name mynginx mynginx:raw
docker container export mynginx | docker image import - mynginx:repack
docker container rm mynginx
docker image list
```
![433030521-55a2ca3e-76e4-49ee-895f-65de73e6970c](https://github.com/user-attachments/assets/c75c4637-5f1b-46cc-880d-fb489916579a)

Utilizarea tuturor metodelor

Cream imaginea mynginx:minx utilizând toate metodele de optimizare. Pentru aceasta definim următorul Dockerfile.min:

![433030635-072fd577-e8cb-439c-bafd-d32b01ad197e](https://github.com/user-attachments/assets/df038a37-2ca6-461e-a426-675938bdc23f)

Construim imaginea cu numele mynginx:minx și verificam dimensiunea. Repachetam imaginea mynginx:minx în mynginx:min:

![433030747-ebe768cf-338d-4bdb-a8f9-b38ec249038e](https://github.com/user-attachments/assets/812a9db1-580d-4c88-926d-2c34cb139dbe)

Pornire și testare

Verificați dimensiunea imaginilor:

![433030853-43d94d05-c4d5-4d48-8030-82b1e65ccf1f](https://github.com/user-attachments/assets/ef81477a-9a60-4920-b567-b3ad4458b170)

Răspunsuri la întrebări:
Care metoda de optimizare a imaginilor vi se pare cea mai eficientă?

Utilizarea Alpine ca imagine de bază oferă cea mai semnificativă reducere a dimensiunii. Alpine este mult mai mică decât Ubuntu, ceea ce face ca imaginea finală să fie mult mai compactă. Combinarea tuturor metodelor (Alpine + minimizarea straturilor + curățarea) oferă rezultatul optim.
De ce curățirea cache-ului pachetelor într-un strat separat nu reduce dimensiunea imaginii?

În Docker, fiecare instrucțiune RUN creează un nou strat. Chiar dacă ștergem fișierele într-un strat nou, acestea au fost deja adăugate în straturile anterioare. Dimensiunea totală a imaginii include toate straturile. De aceea, este mai eficient să combinăm instalarea și curățarea într-o singură instrucțiune RUN, astfel încât fișierele cache să nu fie niciodată persistate într-un strat.
Ce este repachetarea imaginii?

Repachetarea imaginii implică exportarea și importarea unui container. Acest proces comprimă toate straturile imaginii într-un singur strat, eliminând potențiale "straturi moarte" și reducând dimensiunea totală. Cu toate acestea, dezavantajul este că pierdem istoria straturilor și capacitatea de a utiliza cache-ul la reconstruirea imaginii.
Concluzii

Optimizarea imaginilor Docker este esențială pentru reducerea costurilor de stocare, îmbunătățirea vitezei de descărcare și a performanței generale. Cele mai eficiente metode de optimizare sunt:

Utilizarea unei imagini de bază minime (Alpine) Combinarea comenzilor pentru a reduce numărul de straturi Curățarea cache-urilor și a fișierelor temporare în aceeași instrucțiune RUN în care au fost create

Aceste tehnici pot reduce semnificativ dimensiunea imaginilor Docker, făcându-le mai eficiente și mai rapide pentru deployment.
