# fintoc-transfers-high-amount
Script para dividir y ejecutar transferencias de alto monto usando Fintoc Transfers (modo test).
Este repositorio contiene un notebook en Python que simula el uso del producto Transfers de Fintoc en modo test para resolver el problema de realizar transferencias de alto monto de forma automática. El caso parte de la necesidad de transferir $500.000.000 CLP a un solo destinatario, considerando que en Chile existe un límite máximo de $7.000.000 por transferencia, lo que hace inviable hacerlo manualmente.

La idea es tomar un monto total a transferir y los datos de un destinatario, dividir ese monto en la cantidad mínima de transferencias necesarias respetando el límite permitido y ejecutar todas las transferencias programáticamente usando la API de Transfers de Fintoc.

El notebook comienza autenticándose contra la API de Fintoc en modo test, utilizando una API Key y una llave privada JWS generada localmente. Con esta autenticación se obtiene la cuenta de origen desde la cual se realizarán las transferencias. A partir de ahí, el código calcula automáticamente cuántas transferencias son necesarias para completar el monto total, generando en este caso 72 transferencias, donde las primeras 71 son de $7.000.000 y la última corresponde al remanente.

Cada transferencia se crea hacia el mismo destinatario, incluyendo un comentario único que identifica el run y el número de batch, lo que permite luego hacer seguimiento y validaciones. Una vez creadas todas las transferencias, el script consulta la API de forma periódica para revisar el estado de cada una, esperando hasta que todas dejen de estar en estado pending y pasen a un estado final.

Durante este proceso se manejan posibles duplicados que puedan aparecer en la API y se valida que no falte ningún batch, que la cantidad de transferencias sea exactamente la esperada y que la suma total de los montos coincida con los $500.000.000 originales. El objetivo es asegurar que el proceso completo sea consistente y confiable.

Al finalizar, el notebook genera un archivo CSV local con el resultado final del proceso, donde cada fila representa una transferencia y se incluye información como el identificador de la transferencia, el monto, la moneda, el estado final y las fechas relevantes. El script además valida explícitamente que todas las transferencias hayan terminado exitosamente antes de dar el proceso por cerrado.

