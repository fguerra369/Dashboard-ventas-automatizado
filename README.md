# Dashboard-ventas-automatizado
Dashboard automatizado desarrollado con Python y Streamlit para el análisis de ventas mensuales y distribución de clientes por región. Incluye visualizaciones interactivas con Plotly y conexión a datos simulados de negocio.

# Dashboard de Ventas Automatizado

Proyecto en Python + Streamlit que automatiza la generación de dashboards interactivos de ventas.
Incluye integración con Excel y visualizaciones dinámicas.

## 🚀 Tecnologías
- Python
- Streamlit
- Pandas
- Plotly



import streamlit as st
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go


#Configuracion de la pagina
st.set_page_config(page_title="Mi dashboard",
    layout="wide",
    page_icon=":bar_chart:")

data = { 
        'Mes': ['Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo', 'Junio'],
        'Ventas': [1500, 1800, 1700, 2000, 2200, 2100],
        'Clientes': [300, 350, 320, 400, 450, 430],
        'Region': ['Norte', 'Sur', 'Norte', 'Centro',  'Sur', 'Norte']
        }
df = pd.DataFrame(data)

st.title("Dashboard Ejecutivo :bar_chart: ")
st.markdown("### Análisis en tiempo Real de Performance ")

col1, col2,col3, col4 = st.columns(4)

with col1:
    st.metric("Ventas Totales", f"${df['Ventas'].sum():,.0f}", "+5%")
with col2:
    st.metric("Clientes Activos", f"{df['Clientes'].sum():,.0f}", "23")
with col3:
    st.metric("Promedio mensual", f"${df['Ventas'].mean():,.0f}", "8%")  
with col4:  
    st.metric("Crecimiento Anual", "15%", "2%")  
    
fig = px.line(df, x='Mes', y= 'Ventas',
              title='Evolucion de Ventas Mensuales',
              markers=True)
fig.update_layout(template='plotly_dark')
st.plotly_chart(fig, use_container_width=True)

fig2 = px.pie(df, values='Clientes', names='Region',
              title='Distribucion de Clientes por Region')
fig2.update_layout(template='plotly_dark')
st.plotly_chart(fig2, use_container_width=True)

st.sidebar.header("Filtros")
mes_seleccionado = st.sidebar.selectbox("Seleciona un mes:", df['Mes'])
region_filtro = st.sidebar.multiselect("Filtrar por región:", df['Region'].unique())

if  region_filtro:
    datos_filtrados = df[df['Region'].isin(region_filtro)]
    st.dataframe(datos_filtrados, use_container_width=True)
