**Gestión de Identidad (MFA y Gestores) con el Cifrado de Datos**

**Objetivo:** Construir un entorno donde tus accesos estén protegidos y tus archivos mas sensibles permanezcan inaccesibles incluso si pierdes el control físico de tu computadora.

**Paso 1: Configuración de la Bóveda de Identidad**

**Herramientas Sugeridas:**

- **Bitwarden**
- **KeePassXC**
- **VeraCrypt**
- **Elige tu herramienta:** Instala Bitwarden o KeePassXC según tu preferencia (nube o local).
- **Contraseña Maestra Robusta:** Crea una contraseña que sea una frase (mínimo 4 palabras aleatorias). **¡No la olvides!**
- **Carga de Credenciales:** Crea al menos 3 registros de ejemplo. Usa el generador de contraseñas para que tengan al menos 16 caracteres, incluyendo símbolos y números.
- **Protección Adicional:**
  - Si usas **Bitwarden**: Activa el MFA (preferiblemente usando una app como Authy o Google Authenticator).
  - Si usas **KeePassXC**: Configura un "Archivo Llave" (Key File) adicional a tu contraseña.

**Justificaciones:**

- Contraseña **Maestra basada en una frase (mínimo 4 palabras aleatorias):** El uso de frases (passphrases) incrementa significativamente la entropía (longitud real del secreto), lo que hace matemáticamente inviables los ataques de fuerza bruta, manteniendo al mismo tiempo la facilidad de memorización para el usuario.
- **Generación de credenciales de al menos 16 caracteres (letras, números y símbolos):** Asegura una alta complejidad criptográfica para cada servicio individual, mitigando los ataques de diccionario y eliminando por completo la vulnerabilidad asociada a la reutilización de contraseñas.
- **Activación de MFA (Bitwarden) o Archivo Llave (KeePassXC):** Implementa la Autenticación de Múltiples Factores (MFA). Introduce un factor de "posesión" (algo que el usuario tiene, como su teléfono con Authy o el archivo físico), lo que garantiza que si el factor de "conocimiento" (contraseña maestra) se ve comprometido, la bóveda sigue siendo inaccesible.

**Paso 2: Creación del Contenedor Cifrado con VeraCrypt**

1. Abre VeraCrypt y selecciona **"Create Volume"** -> **"Create an encrypted file container"**.
2. Sigue el asistente para un volumen estándar. Ubícalo en una carpeta fácil de encontrar.
3. **Tamaño:** Asígnale **100 MB**.
4. **Cifrado:** Deja los algoritmos por defecto (AES y SHA-512) pero observa las opciones.
5. **Contraseña:** Usa una frase de contraseña distinta a la de tu gestor.
6. **Formateo:** Mueve el ratón aleatoriamente dentro de la ventana para aumentar la fuerza criptográfica (entropía) y finaliza el proceso.

**Justificaciones:**

- **Algoritmos por defecto (AES y SHA-512):** AES (Advanced Encryption Standard) es un algoritmo de cifrado simétrico estándar en la industria, altamente seguro y eficiente. SHA-512 actúa como una función hash criptográfica extremadamente robusta para el proceso de derivación de la clave de cifrado (Key Derivation Function), haciéndola resistente a colisiones.
- **Contraseña del contenedor distinta a la del gestor:** Aplica el principio de seguridad conocido como "compartimentación" o defensa en profundidad. Previene el efecto de compromiso en cascada: si la bóveda de contraseñas es vulnerada, el contenedor local cifrado permanecerá intacto.
- **Movimientos aleatorios del ratón (Entropía):** Los sistemas informáticos son deterministas y carecen de verdadera aleatoriedad. El movimiento humano del ratón recolecta entropía (datos verdaderamente aleatorios e impredecibles) que se utiliza como "semilla" para generar las claves maestras de cifrado, fortaleciendo el volumen contra ataques de criptoanálisis

**Paso 3: Uso y Documentación**

1. **Montar:** Selecciona una letra de unidad (ej. Z:), busca tu archivo recién creado (.hc) y dale a "Mount" ingresando tu contraseña.
2. **Acción:** Dentro de la nueva unidad que aparecerá en tu sistema, crea un archivo llamado aprendizajes.txt y escribe tres cosas clave que aprendiste en esta unidad.
3. **Desmontar:** En VeraCrypt, haz clic en "Dismount". El archivo desaparecerá de tu explorador.

**Justificaciones:**

- **Montaje en una letra de unidad y uso (ej. Z: y creación de aprendizajes.txt):** Permite el cifrado "sobre la marcha" (on-the-fly encryption). Los datos se descifran temporalmente en la memoria RAM solo cuando son leídos, asegurando que nunca se escriban en el disco en formato plano (texto claro).
- **Desmontar (Dismount):** Es un paso crítico de seguridad. Al desmontar el volumen, VeraCrypt purga las claves de descifrado almacenadas temporalmente en la memoria RAM del sistema operativo. Esto mitiga ataques de volcado de memoria (como los _Cold Boot Attacks_ o el uso de malware avanzado) y devuelve el archivo contenedor .hc a su estado de "datos en reposo" cifrados de forma ininteligible.