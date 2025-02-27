

# Titulo

Prompt para evaluar ideas de negocios

# Resumen

El evaluador es un proyecto que busca resolver la falta de una metodología clara y automatizada para validar propuestas de negocio, especialmente en las etapas iniciales de desarrollo. Se enfoca en asistir a emprendedores y startups en el análisis de estudios de mercado, diseño de publicidad y factibilidad financiera. La solución propuesta se basa en el diseño de un prompt de ingeniería que, al alimentarse con información clave del negocio, la ubicación del mercado y la experiencia del ejecutor, permitirá tomar decisiones estratégicas y desarrollar una identidad de marca. El proyecto se implementará en tres etapas de tres semanas cada una, enfocándose en el diseño de prompts, desarrollo de un modelo de texto a imagen para la identidad visual, y pruebas finales para garantizar la efectividad de la solución.

# Problema a abordar 

El problema que se busca resolver es la falta de una metodología clara y automatizada para validar propuestas de negocio, especialmente en las etapas iniciales de desarrollo. Las startups y emprendedores enfrentan dificultades al intentar analizar tres aspectos críticos: estudios de mercado, diseño de publicidad y factibilidad financiera.
Este problema es relevante porque muchos negocios fracasan en sus primeros años debido a decisiones basadas en intuiciones o datos incompletos, lo que resulta en pérdidas económicas y de tiempo. En Colombia, alrededor del 70% de los nuevos negocios no superan los primeros cinco años, según estudios realizados por la Cámara de Comercio de Bogotá y el DANE. Las principales causas de estos fracasos incluyen la falta de planificación estratégica, el desconocimiento del mercado y problemas financieros. La ausencia de un enfoque sistemático limita la capacidad de los emprendedores para tomar decisiones informadas y competir en mercados cada vez más saturados. Desarrollar una solución para este problema puede reducir la incertidumbre y mejorar las probabilidades de éxito de nuevas iniciativas.

# Desarrollo de propuesta

La solución propuesta consiste en diseñar un prompt de ingeniería que permita validar propuestas de negocio al abordar tres áreas clave: estudios de mercado, diseño de publicidad y factibilidad financiera. Además, se busca desarrollar un modelo de texto a imagen que ayude a crear la identidad de marca basándose en los resultados obtenidos del prompt en la sección de estudios de mercado. Este modelo permitirá generar elementos visuales como logotipos, colores y estilos gráficos que reflejen las preferencias y características del público objetivo.


El prompt se estructura de la siguiente manera:

A. Estudio de mercado: Mediante inputs que se le den al modelo como: Idea de negocio, localización del mercado, experiencia del ejecutor, se deberá realizar un estudio de mercado que le permita al usuario tomar decisiones estratégicas para iniciar su negocio.

B. Necesidades de operación: Determinar requerimientos de recursos para iniciar el proyecto y así realizar Factibilidad financiera

C. Diseño de publicidad: De acuerdo a los resultados del estudio de mercado, y a la selección del mercado objetivo se deberá dar una estrategia de publicidad para un año del negocio a iniciar.

D. Factibilidad financiera: A partir del estudio de mercado y sugerencias de inicio, se deberá determinar los costos de capital de trabajo que requeriría el proyecto a un año, esto incluyendo la estrategia de publicidad.

E. Generación de identidad visual: De acuerdo a al estudio de mercado se deberá dar una idea inicial de la identidad de marca: SUgerencia de paleta de colores, sugerencia de diseño de logotipo, así como una descripción de la personalidad de la marca que le permita a un diseñador gráfico perfeccionar la idea.

F. Generaci+on de propuesta de logotipo basado en porpuesta de identidad visual y logotipos de competencia. 

# Justificación de proyecto

El proyecto es técnicamente viable debido a las capacidades actuales de los modelos de IA, como GPT, que pueden procesar información compleja y proporcionar respuestas estructuradas basadas en datos, además de estar conectadas a información pública en linea. La implementación del proyecto seguirá un cronograma dividido en etapas de tres semanas cada una:
- Primera etapa (Semana 1): Diseño inicial de los prompts y configuración del modelo para el análisis de mercado. En esta fase se recopilarán datos relevantes y se realizarán pruebas preliminares para evaluar la calidad de los resultados.

- Segunda etapa (Semana 2): Desarrollo del modelo de texto a imagen y su integración con los resultados obtenidos en el estudio de mercado. Se generarán propuestas visuales iniciales para validar su alineación con las necesidades del público objetivo.

- Tercera etapa (Semana 3): Ajuste y optimización de los prompts y del modelo de generación de imágenes. Se realizarán pruebas finales con distintos casos de negocio para evaluar la efectividad y viabilidad de la solución propuesta.


# Prompt

Import openai
import matplotlib.pyplot as plt
from PIL import Image
import io
import os
import requests
# Configuración de API Key

OPENAI_API_KEY = "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"

openai.api_key = OPENAI_API_KEY
def ejecutar_prompt(prompt: str, temperature: float = 0.7) -> str:
"""
Envía un prompt a la API de OpenAI y devuelve la respuesta de
texto.
"""
try:
response = openai.ChatCompletion.create(
model="gpt-4",
messages=[
{"role": "system", "content": "Eres un asistente
experto en validación de negocios."},
{"role": "user", "content": prompt}
],
temperature=temperature
)
return response["choices"][0]["message"]["content"]
except Exception as e:
return f"Error al ejecutar el prompt: {e}"
def obtener_contenido_url(url: str) -> str:
"""
Descarga el contenido de un documento a partir de una URL.
"""
try:
response = requests.get(url)
response.raise_for_status() # Verifica que la petición fue
exitosa
return response.text
except Exception as e:
return f"Error al obtener contenido desde la URL: {e}"

def generar_logo(prompt: str, n: int = 1, size: str = "1024x1024",
model: str = "dall-e-3") -> str:
"""
Genera una imagen (logo) usando la API de DALL·E y devuelve la URL
de la imagen.
"""
try:
response = openai.Image.create(
prompt=prompt,
n=n,
size=size,
model=model
)
return response["data"][0]["url"]
except Exception as e:
return f"Error generando la imagen: {e}"
# ====================
# Definición de Prompts
# ====================
prompt_estudio_mercado = """
Analiza la viabilidad del siguiente negocio:
Nombre: 'Cafeteria'
Sector: Restaurantería
Ubicación: Colombia
Describe el mercado objetivo, la competencia y las oportunidades de
crecimiento.
"""
prompt_necesidades_operacion = """
Analiza las necesidades de recurso para la puesta en marcha del
negocio.
Crea una lista detallada de necesidades de implementos físicos y
humanos para el inicio de operación del negocio.
"""
prompt_publicidad = """
Genera una estrategia de publicidad digital para la cafetería.
Incluye canales recomendados, presupuesto estimado y un plan de acción
a un año.
"""
prompt_financiero = """
Realiza un análisis financiero para la cafetería.

Incluye costos de capital de trabajo, rentabilidad esperada y punto de
equilibrio de acuerdo a las necesidades de recursos identificadas
anteriormente.
"""
prompt_identidad_visual = """
Sugiere una paleta de colores, logotipo y estilo gráfico para la
cafetería.
Explica cómo estos elementos se alinean con el mercado objetivo.
"""
# Prompt para generar el logo mediante DALL·E
prompt_logo = """
Basándote en las sugerencias de identidad visual proporcionadas,
genera un logo para la cafetería.
El logo debe reflejar modernidad y elegancia, utilizando una paleta de
colores que transmita calidez y profesionalismo.
Toma como ejemplo logotipos de: Juan Valdez, Starbucks y Cedric
Grolet.
"""
# One-shot con URL para documento
url_documento =
"https://docs.google.com/document/d/1b2iZOQhcc1TMMzB6dvRB0bkhoONUMTmoH
1vef2rz1xQ/edit?usp=sharing" # Reemplaza con la URL real del
documento
contenido_url = obtener_contenido_url(url_documento)
prompt_estudio_mercado_url = f"""
Analiza la viabilidad del siguiente negocio basándote en la
información contenida en el documento ubicado en la URL:
{contenido_url}
Por favor, describe el mercado objetivo, la competencia y las
oportunidades de crecimiento.
"""
# ====================
# Ejecución de Prompts y Generación de Resultados
# ====================
if __name__ == "__main__":
# Generar resultados de texto para cada prompt (excepto el logo)
resultados = []
resultados.append("### Estudio de Mercado ###\n")

resultados.append(ejecutar_prompt(prompt_estudio_mercado,
temperature=0.5))
resultados.append("\n### Estudio de Mercado (desde URL) ###\n")
resultados.append(ejecutar_prompt(prompt_estudio_mercado_url,
temperature=0.5))
resultados.append("\n### Necesidades de Operación ###\n")
resultados.append(ejecutar_prompt(prompt_necesidades_operacion,
temperature=0.5))
resultados.append("\n### Estrategia de Publicidad ###\n")
resultados.append(ejecutar_prompt(prompt_publicidad,
temperature=0.5))
resultados.append("\n### Análisis Financiero ###\n")
resultados.append(ejecutar_prompt(prompt_financiero,
temperature=0.5))
resultados.append("\n### Identidad Visual ###\n")
resultados.append(ejecutar_prompt(prompt_identidad_visual,
temperature=0.5))
# Concatenar todos los resultados de texto en una sola cadena
resultados_text = "\n".join(resultados)
# Mostrar directamente el resultado de los textos
print("Resultados de textos:")
print(resultados_text)
# Generar el logo usando DALL·E y obtener la URL de la imagen
url_imagen = generar_logo(prompt_logo, n=1, size="1024x1024")
print("\nURL para imagen del logo:")
print(url_imagen)


# RESULTADOS DE EJEMPLO

### Estudio de Mercado ###

Mercado Objetivo: 
El mercado objetivo de la cafetería en Colombia podría ser bastante amplio, dada la cultura del café en el país. Puede abarcar desde jóvenes estudiantes y profesionales que buscan un espacio para trabajar o estudiar, hasta adultos y ancianos que buscan un lugar para socializar. Además, también se puede orientar hacia turistas que quieren experimentar la famosa cultura del café de Colombia. 

Competencia:
La competencia en el sector de cafeterías en Colombia es alta, ya que el café es una parte integral de la cultura colombiana. Hay desde pequeñas cafeterías independientes hasta grandes cadenas internacionales como Starbucks. Además, también hay competencia indirecta de otros establecimientos de comida y bebida que también sirven café. Para destacar en este mercado altamente competitivo, la cafetería necesitaría ofrecer algo único, ya sea en términos de la calidad del café, el ambiente de la cafetería, el servicio al cliente, o una combinación de estos.

Oportunidades de Crecimiento:
A pesar de la alta competencia, también hay muchas oportunidades de crecimiento en el sector de las cafeterías en Colombia. El consumo de café está creciendo tanto a nivel nacional como internacional, y la demanda de experiencias de café de alta calidad está en aumento. Además, la creciente tendencia de café de especialidad y gourmet presenta una oportunidad para las cafeterías que pueden ofrecer estos productos. También hay oportunidades para expandirse a otras áreas relacionadas, como la venta de granos de café al por menor o la realización de talleres y eventos relacionados con el café. 

En conclusión, aunque el sector de las cafeterías en Colombia es altamente competitivo, hay muchas oportunidades para aquellos que pueden ofrecer una experiencia de café única y de alta calidad. La viabilidad del negocio dependerá en gran medida de su capacidad para diferenciarse de la competencia y captar las tendencias emergentes del mercado.


### Necesidades de Operación ###

Para poder proporcionar una lista detallada de los recursos necesarios, necesitaría conocer qué tipo de negocio se está considerando. Sin embargo, aquí hay una lista general de recursos que cualquier negocio podría necesitar para comenzar:

Recursos físicos:
1. Espacio de oficina o local comercial: El tamaño y la ubicación variarán dependiendo del tipo de negocio.
2. Mobiliario y equipo de oficina: escritorios, sillas, computadoras, impresoras, teléfonos, etc.
3. Equipos especializados: Dependerá del tipo de negocio. Por ejemplo, una cafetería necesitará máquinas de café, mientras que una tienda de ropa necesitará estanterías y percheros.
4. Inventarios: Si se trata de un negocio de venta de productos, necesitará tener un stock inicial.
5. Vehículos: Para la entrega de productos o servicios a domicilio.
6. Materiales de marketing: Tarjetas de visita, folletos, letreros, sitio web, etc.
7. Suministros de oficina: Papel, bolígrafos, clips, etc.
8. Señalización: Para el exterior e interior de tu negocio.
9. Sistemas de seguridad: Cámaras de seguridad, alarmas, etc.

Recursos humanos:
1. Personal de ventas: Para interactuar con los clientes y vender los productos o servicios.
2. Personal de administración: Para manejar las operaciones diarias del negocio, como la contabilidad, la nómina, etc.
3. Personal de marketing: Para promocionar el negocio y atraer a nuevos clientes.
4. Personal de servicio al cliente: Para atender las consultas y problemas de los clientes.
5. Personal de limpieza y mantenimiento: Para mantener el lugar de trabajo limpio y en buen estado.
6. Personal de TI: Para manejar cualquier problema tecnológico que pueda surgir.
7. Personal de logística: Para manejar el inventario y las entregas, si es necesario.
8. Gerencia: Para tomar decisiones estratégicas sobre la dirección del negocio.

Además de estos, también puede haber requisitos financieros, legales y de licencia que necesitarás para poner en marcha tu negocio. Te recomendaría que consultes con un asesor de negocios o un abogado para asegurarte de que estás cumpliendo con todas las regulaciones pertinentes.

### Estrategia de Publicidad ###

Estrategia de Publicidad Digital para Cafetería:

**Objetivo:** Aumentar la visibilidad de la cafetería, atraer a nuevos clientes y fomentar la lealtad de los clientes existentes.

**Canales recomendados:**

1. **Redes Sociales:** Facebook, Instagram, y Twitter son esenciales para cualquier negocio de alimentos y bebidas. Estas plataformas permiten compartir fotos atractivas de los productos, promociones especiales, y eventos. 

2. **Google Ads:** Las campañas de anuncios de Google pueden ayudar a atraer a más clientes locales buscando cafeterías en la zona. 

3. **Email Marketing:** Recopilar direcciones de correo electrónico de los clientes y enviarles newsletters regulares con ofertas especiales, eventos y noticias de la cafetería.

4. **Influencer Marketing:** Colaborar con influencers locales en Instagram para promocionar la cafetería a sus seguidores.

**Presupuesto estimado:** $10,000 al año. Esto puede variar dependiendo de la ubicación y el tamaño de la cafetería. 

**Plan de acción a un año:**

1. **Primer trimestre:** Establecer presencia en las redes sociales y construir una base sólida de seguidores. Comenzar con la recopilación de direcciones de correo electrónico para la campaña de email marketing. Iniciar la primera campaña de Google Ads centrada en atraer a los clientes locales.

2. **Segundo trimestre:** Lanzar la primera campaña de email marketing con ofertas especiales para los suscriptores. Buscar influencers locales para colaboraciones y organizar el primer evento en la cafetería (por ejemplo, una noche de música en vivo o una degustación de café).

3. **Tercer trimestre:** Analizar los resultados de las campañas de publicidad y ajustar la estrategia si es necesario. Continuar con las campañas de email marketing y las colaboraciones con influencers. 

4. **Cuarto trimestre:** Organizar un evento especial para las fiestas y promocionarlo a través de todos los canales de publicidad. Evaluar el éxito de la estrategia de publicidad digital durante el año y planificar la estrategia para el próximo año.

Recuerda que esta estrategia debe ser flexible y ajustarse según los resultados obtenidos. Es importante monitorear constantemente los resultados y hacer ajustes cuando sea necesario.

### Análisis Financiero ###

Para realizar un análisis financiero de una cafetería, necesitamos tener datos específicos. Sin embargo, puedo proporcionarte un esquema general basado en suposiciones y promedios de la industria:

1. **Costos de capital de trabajo:** 
   - **Inventario:** Supongamos que necesitamos mantener un inventario de café, leche, azúcar, productos horneados, etc., que cuesta alrededor de $5,000 al mes.
   - **Salarios:** Supongamos que tienes 5 empleados y pagas un salario promedio de $1,500 por mes a cada uno, lo que suma $7,500.
   - **Alquiler, servicios públicos y otros costos operativos:** Supongamos que estos costos suman otros $5,000 al mes.
   Por lo tanto, los costos de capital de trabajo mensuales suman aproximadamente $17,500.

2. **Rentabilidad esperada:**
   - Supongamos que vendes un promedio de 200 tazas de café al día a un precio de $3 cada una, lo que te da ingresos diarios de $600 y mensuales de $18,000 (considerando un mes de 30 días).
   - Restando los costos de capital de trabajo de tus ingresos, obtienes una rentabilidad mensual de $500.

3. **Punto de equilibrio:**
   - El punto de equilibrio es cuando tus ingresos cubren exactamente tus costos. Dividiendo tus costos fijos mensuales ($17,500) por el precio de venta de una taza de café ($3), necesitas vender alrededor de 5,833 tazas de café al mes para llegar al punto de equilibrio. Eso es aproximadamente 194 tazas de café al día.

Por favor, ten en cuenta que estas cifras son solo suposiciones y pueden variar significativamente dependiendo de tu ubicación, costos de alquiler, salarios, precios de venta, etc. Para un análisis financiero preciso, necesitaría datos específicos de tu cafetería.

### Identidad Visual ###

Paleta de Colores:
Recomendaría una paleta de colores cálidos y acogedores para la cafetería. Esto podría incluir tonos de marrón (que recuerda al café), cremas suaves (que recuerda a la leche o crema), y acentos de naranja y rojo (para agregar un toque de energía y pasión). Estos colores son atractivos, acogedores y pueden fomentar la sensación de comodidad y calidez, alineándose con la experiencia de disfrutar de una taza de café en un ambiente relajado.

Logotipo:
El logotipo podría ser una taza de café estilizada con vapor que sale de ella. Esto es simple, reconocible y directamente relacionado con el producto principal. Podría ser de color marrón oscuro, con el vapor en un tono crema suave. Esto proporciona un buen contraste y es consistente con la paleta de colores.

Estilo Gráfico:
El estilo gráfico podría ser minimalista y moderno, pero con un toque de calidez y rusticidad. Esto podría incluir líneas limpias y formas geométricas simples, pero con texturas que recuerdan a la madera o al papel reciclado. Este estilo es atractivo para una amplia gama de personas, desde jóvenes profesionales hasta personas mayores que buscan un lugar tranquilo para disfrutar de su café.

Estos elementos se alinean con el mercado objetivo porque sugieren una experiencia de café de alta calidad en un ambiente acogedor y relajado. Los colores cálidos y acogedores, el logotipo relacionado con el café y el estilo gráfico minimalista y rústico pueden atraer a personas que valoran tanto la calidad del café como la experiencia de beberlo en un lugar agradable.

![Captura de pantalla 2025-02-27 a la(s) 3 45 37 p m](https://github.com/user-attachments/assets/c0c223d2-4eaa-46dd-aee3-a41b3ef483e4)
