#  Configuración y pruebas de acceso

Repositorio: [https://github.com/Fr4nJG/Seguridad](https://github.com/HoliRabioli/prueba/tree/main)



## 1. Creación del repositorio

Se creó el repositorio `Seguridad` el `.gitignore` y el `SECURITY.md`.

[CAPTURA]

Además, se publicó con GitHub Pages desde la rama `main` para ver el resultado en `https://fr4njg.github.io/Seguridad/`, donde aparece el mensaje **Hola!**.

[CAPTURA]

---

## 2. Invitación al colaborador

Se invitó al compañero desde **Settings → Collaborators and teams → Add people**.

[CAPTURA]

---

## 3. Prueba con permiso de lectura (Read)

El colaborador tiene el rol **Read**. Puede ver el código y comentar, pero no puede modificar el repositorio.

Vista del colaborador con rol de lectura:

[CAPTURA]

El colaborador no puede acceder a **Settings**:

[CAPTURA]

---

## 4. Prueba con permiso de escritura (Write)

Se cambió el rol del colaborador a **Write**.

Rol del colaborador como escritor:

[CAPTURA]

El colaborador añade un archivo al repositorio:

[CAPTURA]

El colaborador sigue sin poder entrar en **Settings**:

[CAPTURA]

---

## 5. Prueba con permiso de administrador (Admin)

Se cambió el rol del colaborador a **Admin**.

Rol del colaborador como administrador:

[CAPTURA]

El colaborador ya puede entrar en **Settings**:

[CAPTURA]

---

## 6. Resumen de resultados

| Prueba | Rol | Resultado observado |
|---|---|---|
| Ver el código y comentar | Read | Permitido |
| Guardar cambios en el repositorio | Read | No permitido |
| Entrar en Settings | Read | No permitido |
| Añadir un archivo | Write | Permitido |
| Entrar en Settings | Write | No permitido |
| Entrar en Settings | Admin | Permitido |

## 7. Conclusión

El nivel de permiso condiciona lo que cada persona puede hacer: **Read** sirve para consultar, **Write** para aportar código y **Admin** para gestionar el repositorio. Se recomienda dar siempre el permiso mínimo necesario. El detalle de roles y medidas de seguridad está en `SECURITY.md`.
