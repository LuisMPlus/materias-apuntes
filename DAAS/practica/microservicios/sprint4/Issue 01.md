# **📋 Plan Técnico de Ejecución — Épica 01: Identidad, Keycloak y RBAC**

**Épica Padre:** `security-service#27` — _Épica 01: Identidad, Keycloak y RBAC_  
**Responsable:** Luis Carlos Manuel Mamaní ([@LuisMPlus](https://github.com/LuisMPlus))  
**Restricción activa:** No realizar Pull Request; trabajar en ramas de desarrollo locales y documentar el paso a paso antes de ejecutar.

---

## **🎯 Objetivos de la Épica**

Migrar la arquitectura de autenticación y autorización desde un esquema simétrico artesanal hacia un servidor industrial de **IAM (Keycloak 26.x)** basado en **OpenID Connect (OIDC)** y criptografía asimétrica **RS256 (JWKS)**, garantizando:

1. Despliegue automatizado del contenedor Keycloak y carga automática (_auto-import_) del Realm `lazyhawk`.
2. Modelado de roles canónicos (`ADMIN`, `VENDEDOR`, `CLIENTE`, `REPARTIDOR`) y usuarios de prueba precargados.
3. Validación de tokens en el **API Gateway** mediante claves públicas (JWKS) sin compartir secretos simétricos.
4. Verificación de **pertenencia de recurso (_Resource Ownership_)** en microservicios, impidiendo que usuarios operen recursos ajenos.
5. Plan de transición documentado para `security-service` (ADR).
6. Batería de pruebas automatizadas y manuales de roles, expiración y códigos `401/403`.

---

## **🗺️ Matriz de Tareas y Dependencias**

![Mermaid diagram](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA3NDcuNzg2OTk5OTk5OTk5OSA3NDUiIHdpZHRoPSI3NDcuNzg2OTk5OTk5OTk5OSIgaGVpZ2h0PSI3NDUiIHN0eWxlPSItLWJnOiMxODE4MUI7LS1mZzojRjRGNEY1Oy0tbGluZTojNzE3MTdBOy0tbXV0ZWQ6I0ExQTFBQTstLXN1cmZhY2U6IzI3MjcyQTstLWJvcmRlcjojM0YzRjQ2O2JhY2tncm91bmQ6dmFyKC0tYmcpIj4KPHN0eWxlPgogIEBpbXBvcnQgdXJsKCdodHRwczovL2ZvbnRzLmdvb2dsZWFwaXMuY29tL2NzczI/ZmFtaWx5PUludGVyOndnaHRANDAwOzUwMDs2MDA7NzAwJmFtcDtkaXNwbGF5PXN3YXAnKTsKICB0ZXh0IHsgZm9udC1mYW1pbHk6ICdJbnRlcicsIHN5c3RlbS11aSwgc2Fucy1zZXJpZjsgfQogIHN2ZyB7CiAgICAvKiBEZXJpdmVkIGZyb20gLS1iZyBhbmQgLS1mZyAob3ZlcnJpZGFibGUgdmlhIC0tbGluZSwgLS1hY2NlbnQsIGV0Yy4pICovCiAgICAtLV90ZXh0OiAgICAgICAgICB2YXIoLS1mZyk7CiAgICAtLV90ZXh0LXNlYzogICAgICB2YXIoLS1tdXRlZCwgY29sb3ItbWl4KGluIHNyZ2IsIHZhcigtLWZnKSA2MCUsIHZhcigtLWJnKSkpOwogICAgLS1fdGV4dC1tdXRlZDogICAgdmFyKC0tbXV0ZWQsIGNvbG9yLW1peChpbiBzcmdiLCB2YXIoLS1mZykgNDAlLCB2YXIoLS1iZykpKTsKICAgIC0tX3RleHQtZmFpbnQ6ICAgIGNvbG9yLW1peChpbiBzcmdiLCB2YXIoLS1mZykgMjUlLCB2YXIoLS1iZykpOwogICAgLS1fbGluZTogICAgICAgICAgdmFyKC0tbGluZSwgY29sb3ItbWl4KGluIHNyZ2IsIHZhcigtLWZnKSA1MCUsIHZhcigtLWJnKSkpOwogICAgLS1fYXJyb3c6ICAgICAgICAgdmFyKC0tYWNjZW50LCBjb2xvci1taXgoaW4gc3JnYiwgdmFyKC0tZmcpIDg1JSwgdmFyKC0tYmcpKSk7CiAgICAtLV9ub2RlLWZpbGw6ICAgICB2YXIoLS1zdXJmYWNlLCBjb2xvci1taXgoaW4gc3JnYiwgdmFyKC0tZmcpIDMlLCB2YXIoLS1iZykpKTsKICAgIC0tX25vZGUtc3Ryb2tlOiAgIHZhcigtLWJvcmRlciwgY29sb3ItbWl4KGluIHNyZ2IsIHZhcigtLWZnKSAyMCUsIHZhcigtLWJnKSkpOwogICAgLS1fZ3JvdXAtZmlsbDogICAgdmFyKC0tYmcpOwogICAgLS1fZ3JvdXAtaGRyOiAgICAgY29sb3ItbWl4KGluIHNyZ2IsIHZhcigtLWZnKSA1JSwgdmFyKC0tYmcpKTsKICAgIC0tX2lubmVyLXN0cm9rZTogIGNvbG9yLW1peChpbiBzcmdiLCB2YXIoLS1mZykgMTIlLCB2YXIoLS1iZykpOwogICAgLS1fa2V5LWJhZGdlOiAgICAgY29sb3ItbWl4KGluIHNyZ2IsIHZhcigtLWZnKSAxMCUsIHZhcigtLWJnKSk7CiAgfQo8L3N0eWxlPgo8ZGVmcz4KICA8bWFya2VyIGlkPSJhcnJvd2hlYWQiIG1hcmtlcldpZHRoPSI4IiBtYXJrZXJIZWlnaHQ9IjUiIHJlZlg9IjciIHJlZlk9IjIuNSIgb3JpZW50PSJhdXRvIj4KICAgIDxwb2x5Z29uIHBvaW50cz0iMCAwLCA4IDIuNSwgMCA1IiBmaWxsPSJ2YXIoLS1fYXJyb3cpIiBzdHJva2U9InZhcigtLV9hcnJvdykiIHN0cm9rZS13aWR0aD0iMC43NSIgc3Ryb2tlLWxpbmVqb2luPSJyb3VuZCIgLz4KICA8L21hcmtlcj4KICA8bWFya2VyIGlkPSJhcnJvd2hlYWQtc3RhcnQiIG1hcmtlcldpZHRoPSI4IiBtYXJrZXJIZWlnaHQ9IjUiIHJlZlg9IjEiIHJlZlk9IjIuNSIgb3JpZW50PSJhdXRvLXN0YXJ0LXJldmVyc2UiPgogICAgPHBvbHlnb24gcG9pbnRzPSI4IDAsIDAgMi41LCA4IDUiIGZpbGw9InZhcigtLV9hcnJvdykiIHN0cm9rZT0idmFyKC0tX2Fycm93KSIgc3Ryb2tlLXdpZHRoPSIwLjc1IiBzdHJva2UtbGluZWpvaW49InJvdW5kIiAvPgogIDwvbWFya2VyPgo8L2RlZnM+CjxnIGNsYXNzPSJzdWJncmFwaCIgZGF0YS1pZD0iSU5GUkEiIGRhdGEtbGFiZWw9IiBsb2dpdHJhY2staW5mcmEgIj4KICA8cmVjdCB4PSIxOTkuMDkyNDk5OTk5OTk5OTciIHk9IjQwIiB3aWR0aD0iMzU1LjUzIiBoZWlnaHQ9IjIxNS42MDAwMDAwMDAwMDAwMiIgcng9IjAiIHJ5PSIwIiBmaWxsPSJ2YXIoLS1fZ3JvdXAtZmlsbCkiIHN0cm9rZT0idmFyKC0tX25vZGUtc3Ryb2tlKSIgc3Ryb2tlLXdpZHRoPSIxIiAvPgogIDxyZWN0IHg9IjE5OS4wOTI0OTk5OTk5OTk5NyIgeT0iNDAiIHdpZHRoPSIzNTUuNTMiIGhlaWdodD0iMjgiIHJ4PSIwIiByeT0iMCIgZmlsbD0idmFyKC0tX2dyb3VwLWhkcikiIHN0cm9rZT0idmFyKC0tX25vZGUtc3Ryb2tlKSIgc3Ryb2tlLXdpZHRoPSIxIiAvPgogIDx0ZXh0IHg9IjIxMS4wOTI0OTk5OTk5OTk5NyIgeT0iNTQiIGZvbnQtc2l6ZT0iMTIiIGZvbnQtd2VpZ2h0PSI2MDAiIGZpbGw9InZhcigtLV90ZXh0LXNlYykiIGR5PSI0LjE5OTk5OTk5OTk5OTk5OSI+IGxvZ2l0cmFjay1pbmZyYSA8L3RleHQ+CjwvZz4KPGcgY2xhc3M9InN1YmdyYXBoIiBkYXRhLWlkPSJHQVRFV0FZIiBkYXRhLWxhYmVsPSIgYXBpLWdhdGV3YXkgIj4KICA8cmVjdCB4PSI0MCIgeT0iMzE1LjYiIHdpZHRoPSIzNTEuMDgzOTk5OTk5OTk5OTUiIGhlaWdodD0iMTEzLjgwMDAwMDAwMDAwMDAxIiByeD0iMCIgcnk9IjAiIGZpbGw9InZhcigtLV9ncm91cC1maWxsKSIgc3Ryb2tlPSJ2YXIoLS1fbm9kZS1zdHJva2UpIiBzdHJva2Utd2lkdGg9IjEiIC8+CiAgPHJlY3QgeD0iNDAiIHk9IjMxNS42IiB3aWR0aD0iMzUxLjA4Mzk5OTk5OTk5OTk1IiBoZWlnaHQ9IjI4IiByeD0iMCIgcnk9IjAiIGZpbGw9InZhcigtLV9ncm91cC1oZHIpIiBzdHJva2U9InZhcigtLV9ub2RlLXN0cm9rZSkiIHN0cm9rZS13aWR0aD0iMSIgLz4KICA8dGV4dCB4PSI1MiIgeT0iMzI5LjYiIGZvbnQtc2l6ZT0iMTIiIGZvbnQtd2VpZ2h0PSI2MDAiIGZpbGw9InZhcigtLV90ZXh0LXNlYykiIGR5PSI0LjE5OTk5OTk5OTk5OTk5OSI+IGFwaS1nYXRld2F5IDwvdGV4dD4KPC9nPgo8ZyBjbGFzcz0ic3ViZ3JhcGgiIGRhdGEtaWQ9IlNFQ1VSSVRZIiBkYXRhLWxhYmVsPSIgc2VjdXJpdHktc2VydmljZSAiPgogIDxyZWN0IHg9IjQzLjcwNSIgeT0iNDg5LjQwMDAwMDAwMDAwMDAzIiB3aWR0aD0iNjY0LjA4MTk5OTk5OTk5OTkiIGhlaWdodD0iMjE1LjYwMDAwMDAwMDAwMDAyIiByeD0iMCIgcnk9IjAiIGZpbGw9InZhcigtLV9ncm91cC1maWxsKSIgc3Ryb2tlPSJ2YXIoLS1fbm9kZS1zdHJva2UpIiBzdHJva2Utd2lkdGg9IjEiIC8+CiAgPHJlY3QgeD0iNDMuNzA1IiB5PSI0ODkuNDAwMDAwMDAwMDAwMDMiIHdpZHRoPSI2NjQuMDgxOTk5OTk5OTk5OSIgaGVpZ2h0PSIyOCIgcng9IjAiIHJ5PSIwIiBmaWxsPSJ2YXIoLS1fZ3JvdXAtaGRyKSIgc3Ryb2tlPSJ2YXIoLS1fbm9kZS1zdHJva2UpIiBzdHJva2Utd2lkdGg9IjEiIC8+CiAgPHRleHQgeD0iNTUuNzA1IiB5PSI1MDMuNDAwMDAwMDAwMDAwMDMiIGZvbnQtc2l6ZT0iMTIiIGZvbnQtd2VpZ2h0PSI2MDAiIGZpbGw9InZhcigtLV90ZXh0LXNlYykiIGR5PSI0LjE5OTk5OTk5OTk5OTk5OSI+IHNlY3VyaXR5LXNlcnZpY2UgPC90ZXh0Pgo8L2c+Cjxwb2x5bGluZSBjbGFzcz0iZWRnZSIgZGF0YS1mcm9tPSJGMDIiIGRhdGEtdG89IkYwMyIgZGF0YS1zdHlsZT0idGhpY2siIGRhdGEtYXJyb3ctc3RhcnQ9ImZhbHNlIiBkYXRhLWFycm93LWVuZD0idHJ1ZSIgcG9pbnRzPSIzMjIuOTM1ODMzMzMzMzMzMywyMzkuNjAwMDAwMDAwMDAwMDIgMzIyLjkzNTgzMzMzMzMzMzMsMjk3LjYgMjE1LjU0MTk5OTk5OTk5OTk3LDI5Ny42IDIxNS41NDE5OTk5OTk5OTk5NywzNTkuNiIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJ2YXIoLS1fbGluZSkiIHN0cm9rZS13aWR0aD0iMiIgbWFya2VyLWVuZD0idXJsKCNhcnJvd2hlYWQpIiAvPgo8cG9seWxpbmUgY2xhc3M9ImVkZ2UiIGRhdGEtZnJvbT0iRjAzIiBkYXRhLXRvPSJGMDQiIGRhdGEtc3R5bGU9InRoaWNrIiBkYXRhLWFycm93LXN0YXJ0PSJmYWxzZSIgZGF0YS1hcnJvdy1lbmQ9InRydWUiIHBvaW50cz0iMjE1LjU0MTk5OTk5OTk5OTk3LDQxMy40MDAwMDAwMDAwMDAwMyAyMTUuNTQxOTk5OTk5OTk5OTcsNDcxLjQwMDAwMDAwMDAwMDAzIDIxMS44MzcsNDcxLjQwMDAwMDAwMDAwMDAzIDIxMS44MzcsNTMzLjQwMDAwMDAwMDAwMDEiIGZpbGw9Im5vbmUiIHN0cm9rZT0idmFyKC0tX2xpbmUpIiBzdHJva2Utd2lkdGg9IjIiIG1hcmtlci1lbmQ9InVybCgjYXJyb3doZWFkKSIgLz4KPHBvbHlsaW5lIGNsYXNzPSJlZGdlIiBkYXRhLWZyb209IkYwMiIgZGF0YS10bz0iRjA1IiBkYXRhLXN0eWxlPSJzb2xpZCIgZGF0YS1hcnJvdy1zdGFydD0iZmFsc2UiIGRhdGEtYXJyb3ctZW5kPSJ0cnVlIiBwb2ludHM9IjQzMC43NzkxNjY2NjY2NjY2LDIzOS42MDAwMDAwMDAwMDAwMiA0MzAuNzc5MTY2NjY2NjY2Niw0NzEuNDAwMDAwMDAwMDAwMDMgNTM4LjE3Mjk5OTk5OTk5OTksNDcxLjQwMDAwMDAwMDAwMDAzIDUzOC4xNzI5OTk5OTk5OTk5LDQ3MS40MDAwMDAwMDAwMDAwMyA1NDEuODc3OTk5OTk5OTk5OSw0NzEuNDAwMDAwMDAwMDAwMDMgNTQxLjg3Nzk5OTk5OTk5OTksNTMzLjQwMDAwMDAwMDAwMDEiIGZpbGw9Im5vbmUiIHN0cm9rZT0idmFyKC0tX2xpbmUpIiBzdHJva2Utd2lkdGg9IjEiIG1hcmtlci1lbmQ9InVybCgjYXJyb3doZWFkKSIgLz4KPHBvbHlsaW5lIGNsYXNzPSJlZGdlIiBkYXRhLWZyb209IkYwMSIgZGF0YS10bz0iRjAyIiBkYXRhLXN0eWxlPSJzb2xpZCIgZGF0YS1hcnJvdy1zdGFydD0iZmFsc2UiIGRhdGEtYXJyb3ctZW5kPSJ0cnVlIiBwb2ludHM9IjM3Ni44NTc0OTk5OTk5OTk5NiwxMzcuOCAzNzYuODU3NDk5OTk5OTk5OTYsMTg1LjgiIGZpbGw9Im5vbmUiIHN0cm9rZT0idmFyKC0tX2xpbmUpIiBzdHJva2Utd2lkdGg9IjEiIG1hcmtlci1lbmQ9InVybCgjYXJyb3doZWFkKSIgLz4KPHBvbHlsaW5lIGNsYXNzPSJlZGdlIiBkYXRhLWZyb209IkYwNCIgZGF0YS10bz0iRjA2IiBkYXRhLXN0eWxlPSJ0aGljayIgZGF0YS1hcnJvdy1zdGFydD0iZmFsc2UiIGRhdGEtYXJyb3ctZW5kPSJ0cnVlIiBwb2ludHM9IjIxMS44MzcsNTg3LjIgMjExLjgzNyw2MzUuMiIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJ2YXIoLS1fbGluZSkiIHN0cm9rZS13aWR0aD0iMiIgbWFya2VyLWVuZD0idXJsKCNhcnJvd2hlYWQpIiAvPgo8ZyBjbGFzcz0ibm9kZSIgZGF0YS1pZD0iRjAxIiBkYXRhLWxhYmVsPSJGMDE6IERlc3BsZWdhciBLZXljbG9hayBlbiBDb21wb3NlCihQdWVydG8gOjgwODAgLyBSZWFsbSBsYXp5aGF3aykiIGRhdGEtc2hhcGU9InJlY3RhbmdsZSI+CiAgPHJlY3QgeD0iMjQ2LjIxNDUiIHk9Ijg0IiB3aWR0aD0iMjYxLjI4NTk5OTk5OTk5OTk0IiBoZWlnaHQ9IjUzLjgwMDAwMDAwMDAwMDAwNCIgcng9IjAiIHJ5PSIwIiBmaWxsPSJ2YXIoLS1fbm9kZS1maWxsKSIgc3Ryb2tlPSJ2YXIoLS1fbm9kZS1zdHJva2UpIiBzdHJva2Utd2lkdGg9IjAuNzUiIC8+CiAgPHRleHQgeD0iMzc2Ljg1NzQ5OTk5OTk5OTk2IiB5PSIxMTAuOSIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMyIgZm9udC13ZWlnaHQ9IjUwMCIgZmlsbD0idmFyKC0tX3RleHQpIj48dHNwYW4geD0iMzc2Ljg1NzQ5OTk5OTk5OTk2IiBkeT0iLTMuOTAwMDAwMDAwMDAwMDAxMiI+RjAxOiBEZXNwbGVnYXIgS2V5Y2xvYWsgZW4gQ29tcG9zZTwvdHNwYW4+PHRzcGFuIHg9IjM3Ni44NTc0OTk5OTk5OTk5NiIgZHk9IjE2LjkwMDAwMDAwMDAwMDAwMiI+KFB1ZXJ0byA6ODA4MCAvIFJlYWxtIGxhenloYXdrKTwvdHNwYW4+PC90ZXh0Pgo8L2c+CjxnIGNsYXNzPSJub2RlIiBkYXRhLWlkPSJGMDIiIGRhdGEtbGFiZWw9IkYwMjogQ29uZmlndXJhciBDbGllbnRlcywgUm9sZXMgeSBVc3VhcmlvcyBTZWVkCihsYXp5aGF3ay1yZWFsbS5qc29uKSIgZGF0YS1zaGFwZT0icmVjdGFuZ2xlIj4KICA8cmVjdCB4PSIyMTUuMDkyNDk5OTk5OTk5OTciIHk9IjE4NS44IiB3aWR0aD0iMzIzLjUzIiBoZWlnaHQ9IjUzLjgwMDAwMDAwMDAwMDAwNCIgcng9IjAiIHJ5PSIwIiBmaWxsPSJ2YXIoLS1fbm9kZS1maWxsKSIgc3Ryb2tlPSJ2YXIoLS1fbm9kZS1zdHJva2UpIiBzdHJva2Utd2lkdGg9IjAuNzUiIC8+CiAgPHRleHQgeD0iMzc2Ljg1NzQ5OTk5OTk5OTk2IiB5PSIyMTIuNzAwMDAwMDAwMDAwMDIiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSI1MDAiIGZpbGw9InZhcigtLV90ZXh0KSI+PHRzcGFuIHg9IjM3Ni44NTc0OTk5OTk5OTk5NiIgZHk9Ii0zLjkwMDAwMDAwMDAwMDAwMTIiPkYwMjogQ29uZmlndXJhciBDbGllbnRlcywgUm9sZXMgeSBVc3VhcmlvcyBTZWVkPC90c3Bhbj48dHNwYW4geD0iMzc2Ljg1NzQ5OTk5OTk5OTk2IiBkeT0iMTYuOTAwMDAwMDAwMDAwMDAyIj4obGF6eWhhd2stcmVhbG0uanNvbik8L3RzcGFuPjwvdGV4dD4KPC9nPgo8ZyBjbGFzcz0ibm9kZSIgZGF0YS1pZD0iRjAzIiBkYXRhLWxhYmVsPSJGMDM6IFZhbGlkYWNpw7NuIEpXVCBBc2ltw6l0cmljbyAoSldLUykKKEZpcm1hLCBJc3N1ZXIsIEF1ZGllbmNpYSB5IE1hcGVvIERvd25zdHJlYW0pIiBkYXRhLXNoYXBlPSJyZWN0YW5nbGUiPgogIDxyZWN0IHg9IjU2IiB5PSIzNTkuNiIgd2lkdGg9IjMxOS4wODM5OTk5OTk5OTk5NSIgaGVpZ2h0PSI1My44MDAwMDAwMDAwMDAwMDQiIHJ4PSIwIiByeT0iMCIgZmlsbD0idmFyKC0tX25vZGUtZmlsbCkiIHN0cm9rZT0idmFyKC0tX25vZGUtc3Ryb2tlKSIgc3Ryb2tlLXdpZHRoPSIwLjc1IiAvPgogIDx0ZXh0IHg9IjIxNS41NDE5OTk5OTk5OTk5NyIgeT0iMzg2LjUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSI1MDAiIGZpbGw9InZhcigtLV90ZXh0KSI+PHRzcGFuIHg9IjIxNS41NDE5OTk5OTk5OTk5NyIgZHk9Ii0zLjkwMDAwMDAwMDAwMDAwMTIiPkYwMzogVmFsaWRhY2nDs24gSldUIEFzaW3DqXRyaWNvIChKV0tTKTwvdHNwYW4+PHRzcGFuIHg9IjIxNS41NDE5OTk5OTk5OTk5NyIgZHk9IjE2LjkwMDAwMDAwMDAwMDAwMiI+KEZpcm1hLCBJc3N1ZXIsIEF1ZGllbmNpYSB5IE1hcGVvIERvd25zdHJlYW0pPC90c3Bhbj48L3RleHQ+CjwvZz4KPGcgY2xhc3M9Im5vZGUiIGRhdGEtaWQ9IkYwNSIgZGF0YS1sYWJlbD0iRjA1OiBBRFIgZGUgVHJhbnNpY2nDs24gZGUgc2VjdXJpdHktc2VydmljZQooQ29leGlzdGVuY2lhIHkgbWlncmFjacOzbiBkZSBwZXJmaWxlcykiIGRhdGEtc2hhcGU9InJlY3RhbmdsZSI+CiAgPHJlY3QgeD0iMzkxLjk2ODk5OTk5OTk5OTk0IiB5PSI1MzMuNDAwMDAwMDAwMDAwMSIgd2lkdGg9IjI5OS44MTc5OTk5OTk5OTk5IiBoZWlnaHQ9IjUzLjgwMDAwMDAwMDAwMDAwNCIgcng9IjAiIHJ5PSIwIiBmaWxsPSJ2YXIoLS1fbm9kZS1maWxsKSIgc3Ryb2tlPSJ2YXIoLS1fbm9kZS1zdHJva2UpIiBzdHJva2Utd2lkdGg9IjAuNzUiIC8+CiAgPHRleHQgeD0iNTQxLjg3Nzk5OTk5OTk5OTkiIHk9IjU2MC4zMDAwMDAwMDAwMDAxIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iNTAwIiBmaWxsPSJ2YXIoLS1fdGV4dCkiPjx0c3BhbiB4PSI1NDEuODc3OTk5OTk5OTk5OSIgZHk9Ii0zLjkwMDAwMDAwMDAwMDAwMTIiPkYwNTogQURSIGRlIFRyYW5zaWNpw7NuIGRlIHNlY3VyaXR5LXNlcnZpY2U8L3RzcGFuPjx0c3BhbiB4PSI1NDEuODc3OTk5OTk5OTk5OSIgZHk9IjE2LjkwMDAwMDAwMDAwMDAwMiI+KENvZXhpc3RlbmNpYSB5IG1pZ3JhY2nDs24gZGUgcGVyZmlsZXMpPC90c3Bhbj48L3RleHQ+CjwvZz4KPGcgY2xhc3M9Im5vZGUiIGRhdGEtaWQ9IkYwNCIgZGF0YS1sYWJlbD0iRjA0OiBDb250cm9sIGRlIFJvbGVzIHkgUmVzb3VyY2UgT3duZXJzaGlwCihWYWxpZGFjacOzbiBkZSBYLUF1dGgtVXNlciB5IFgtQXV0aC1Sb2xlcykiIGRhdGEtc2hhcGU9InJlY3RhbmdsZSI+CiAgPHJlY3QgeD0iNTkuNzA1IiB5PSI1MzMuNDAwMDAwMDAwMDAwMSIgd2lkdGg9IjMwNC4yNjM5OTk5OTk5OTk5NSIgaGVpZ2h0PSI1My44MDAwMDAwMDAwMDAwMDQiIHJ4PSIwIiByeT0iMCIgZmlsbD0idmFyKC0tX25vZGUtZmlsbCkiIHN0cm9rZT0idmFyKC0tX25vZGUtc3Ryb2tlKSIgc3Ryb2tlLXdpZHRoPSIwLjc1IiAvPgogIDx0ZXh0IHg9IjIxMS44MzciIHk9IjU2MC4zMDAwMDAwMDAwMDAxIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEzIiBmb250LXdlaWdodD0iNTAwIiBmaWxsPSJ2YXIoLS1fdGV4dCkiPjx0c3BhbiB4PSIyMTEuODM3IiBkeT0iLTMuOTAwMDAwMDAwMDAwMDAxMiI+RjA0OiBDb250cm9sIGRlIFJvbGVzIHkgUmVzb3VyY2UgT3duZXJzaGlwPC90c3Bhbj48dHNwYW4geD0iMjExLjgzNyIgZHk9IjE2LjkwMDAwMDAwMDAwMDAwMiI+KFZhbGlkYWNpw7NuIGRlIFgtQXV0aC1Vc2VyIHkgWC1BdXRoLVJvbGVzKTwvdHNwYW4+PC90ZXh0Pgo8L2c+CjxnIGNsYXNzPSJub2RlIiBkYXRhLWlkPSJGMDYiIGRhdGEtbGFiZWw9IkYwNjogQmF0ZXLDrWEgZGUgUHJ1ZWJhcyBkZSBTZWd1cmlkYWQKKDIwMCBPSywgNDAxIFVuYXV0aG9yaXplZCwgNDAzIEZvcmJpZGRlbikiIGRhdGEtc2hhcGU9InJlY3RhbmdsZSI+CiAgPHJlY3QgeD0iNjcuNDg1NSIgeT0iNjM1LjIiIHdpZHRoPSIyODguNzAzIiBoZWlnaHQ9IjUzLjgwMDAwMDAwMDAwMDAwNCIgcng9IjAiIHJ5PSIwIiBmaWxsPSJ2YXIoLS1fbm9kZS1maWxsKSIgc3Ryb2tlPSJ2YXIoLS1fbm9kZS1zdHJva2UpIiBzdHJva2Utd2lkdGg9IjAuNzUiIC8+CiAgPHRleHQgeD0iMjExLjgzNyIgeT0iNjYyLjEiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTMiIGZvbnQtd2VpZ2h0PSI1MDAiIGZpbGw9InZhcigtLV90ZXh0KSI+PHRzcGFuIHg9IjIxMS44MzciIGR5PSItMy45MDAwMDAwMDAwMDAwMDEyIj5GMDY6IEJhdGVyw61hIGRlIFBydWViYXMgZGUgU2VndXJpZGFkPC90c3Bhbj48dHNwYW4geD0iMjExLjgzNyIgZHk9IjE2LjkwMDAwMDAwMDAwMDAwMiI+KDIwMCBPSywgNDAxIFVuYXV0aG9yaXplZCwgNDAzIEZvcmJpZGRlbik8L3RzcGFuPjwvdGV4dD4KPC9nPgo8L3N2Zz4=)

---

## **🛠️ Plan Detallado Paso a Paso**

### **Paso 1: [F01] Desplegar Keycloak en `logitrack-infra`**

- **Rama:** `feature/f01-keycloak-compose` (en `logitrack-infra`)
- **Acciones:**
    1. Agregar el servicio `keycloak` a `docker-compose.yml`:
        - Imagen: `quay.io/keycloak/keycloak:26.0` (o compatible con Java 21/25).
        - Comando: `start-dev --import-realm`.
        - Puerto expuesto: `8080:8080`.
        - Variables de entorno: `KEYCLOAK_ADMIN`, `KEYCLOAK_ADMIN_PASSWORD`, base de datos o almacenamiento H2 dev para inicio ultra-rápido.
        - Volumen: Montar la carpeta `./keycloak/realm` hacia `/opt/keycloak/data/import` para importación automática en arranque.
        - Healthcheck: Verificación de endpoint `/realms/master` o `/health/ready`.
    2. Conectar a `logitrack-network` para comunicación interna con el Gateway.

---

### **Paso 2: [F02] Modelar Realm, Clientes, Roles y Usuarios Semilla**

- **Archivo:** `logitrack-infra/keycloak/realm/lazyhawk-realm.json`
- **Acciones:**
    1. **Realm:** `lazyhawk`.
    2. **Roles de Realm:**
        - `ADMIN`: Acceso total a dashboards, reasignaciones y todos los recursos del ecosistema.
        - `VENDEDOR`: Gestión del catálogo de comercio (alta, modificación de precios/stock y eliminación de productos en `commerce-service`).
        - `CLIENTE`: Navegación de productos, carrito, compras y seguimiento de sus propios envíos.
        - `REPARTIDOR`: Consulta de envíos asignados y actualización de estado de entrega en ruta.
    3. **Clientes:**
        - `logitrack-frontend`: Cliente público (SPA en React), `standardFlowEnabled: true`, `implicitFlowEnabled: false`, `pkceCodeChallengeMethod: S256`, web origins: `*` o `http://localhost:3000`.
        - `logitrack-api`: Cliente bearer-only para verificación de recursos.
    4. **Protocol Mappers:**
        - Mapear roles a claim raíz `roles` o `realm_access.roles` dentro del Access Token JWT.
    5. **Usuarios de Prueba (Seed inicial):**
        - `admin@logitrack.com` →\rightarrow→ Rol `ADMIN` (password: `admin123`).
        - `vendedor@logitrack.com` →\rightarrow→ Rol `VENDEDOR` (password: `vendedor123`).
        - `cliente@logitrack.com` →\rightarrow→ Rol `CLIENTE` (password: `cliente123`).
        - `repartidor@logitrack.com` →\rightarrow→ Rol `REPARTIDOR` (password: `repartidor123`).

---

### **Paso 3: [F03] Configurar JWT de Keycloak en `api-gateway`**

- **Rama:** `feature/f03-gateway-keycloak-jwt` (en `api-gateway`)
- **Acciones:**
    1. **Validación Criptográfica Asimétrica (JWKS):**
        - Actualizar la validación de tokens en `JwtUtil` / `JwtAuthenticationFilter`:
        - En lugar de la clave simétrica (`security.jwt.secret-key`), configurar la URL pública de certificados de Keycloak: `http://keycloak:8080/realms/lazyhawk/protocol/openid-connect/certs`
        - Validar emisor (`iss: http://localhost:8080/realms/lazyhawk` o internal host), firma RS256 y vigencia (`exp`).
    2. **Inyección de Cabeceras Downstream:**
        - Extraer el identificador de usuario (`sub` o `preferred_username` / `email`) y propagarlo en la cabecera `X-Auth-User`.
        - Extraer los roles de Keycloak y propagarlos en `X-Auth-Roles` (ej. `ROLE_ADMIN`, `ROLE_CLIENTE`).
    3. **Rutas Públicas vs Protegidas:**
        - Mantener libres las peticiones `OPTIONS` (CORS) y el `/actuator/health`.
        - Proteger todas las rutas de negocio bajo `/api/v1/**`.

---

### **Paso 4: [F05] Planificar Transición de `security-service` (ADR)**

- **Archivo:** `security-service/docs/adr/ADR-003-transicion-keycloak.md`
- **Acciones:**
    1. Redactar el Registro de Decisión Arquitectónica (ADR):
        - **Contexto:** `security-service` gestionaba contraseñas y emitía JWTs propios.
        - **Decisión:** Delegar autenticación, recuperación de credenciales y emisión OIDC a Keycloak.
        - **Rol futuro de `security-service`:** Pasa a ser un servicio de perfiles de usuario y metadatos (`user-service`), o se mantiene como fachada (_adapter_) mientras dure la transición de clientes antiguos.
        - **Mapeo de identidades:** El campo `email` y `keycloakId` actuarán como puente entre Keycloak y las tablas locales.

---

### **Paso 5: [F04] Proteger APIs por Rol y Resource Ownership**

- **Rama:** `feature/f04-rbac-ownership` (en `security-service`)
- **Acciones:**
    1. Validar las cabeceras `X-Auth-User` y `X-Auth-Roles` en los controladores:
        - Si el rol es `ADMIN`, tiene autorización para cualquier ID.
        - Si el rol es `CLIENTE`, solo puede acceder si el `id` solicitado coincide con su propio `X-Auth-User`.
    2. Si intenta acceder a un recurso ajeno: responder con código **HTTP `403 Forbidden`** (_"Acceso denegado: No posee pertenencia sobre este recurso"_).

---

### **Paso 6: [F06] Batería de Pruebas de Roles, Expiración y 401/403**

- **Acciones:**
    1. **Pruebas Automatizadas (JUnit / MockMvc):**
        - Test de acceso sin token →\rightarrow→ `401 Unauthorized`.
        - Test de token con firma inválida o expirado →\rightarrow→ `401 Unauthorized`.
        - Test de `CLIENTE` intentando acceder a endpoint de `ADMIN` →\rightarrow→ `403 Forbidden`.
        - Test de `CLIENTE A` consultando recurso de `CLIENTE B` →\rightarrow→ `403 Forbidden`.
        - Test de `ADMIN` consultando cualquier recurso →\rightarrow→ `200 OK`.
    2. **Script de Verificación en Postman / Apidog / curl:**
        - Flujo de obtención de token contra Keycloak (`POST /protocol/openid-connect/token`).
        - Disparo de peticiones a través del API Gateway (`:8066`).

---

## **🔒 Compromisos y Reglas de Trabajo**

- **Sin Pull Requests:** Todo el trabajo permanecerá en sus respectivas ramas temáticas locales/remotas sin abrir PR hasta indicación expresa.
- **Preservación de Entornos:** Los perfiles existentes de desarrollo y pruebas no se romperán durante la configuración de Keycloak.