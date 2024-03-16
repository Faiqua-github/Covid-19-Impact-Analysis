# Covid-19-Impact-Analysis

### Code for Building Dashboard using Python Dash Libruary

import numpy as np 
import pandas as pd 
import plotly.graph_objs as go 
import dash 
import dash_core_components as dcc 
import dash_html_components as html 
from dash.dependencies import Input, Output 
import plotly.express as px 
 
external_stylesheet= [    
    {      
        'href': 'https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css',         
        'rel': 'stylesheet',         
        'integrity': 'sha384Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh',         
        'crossorigin': 'anonymous'    
     } 
] 

#patients=pd.read_csv('state_wise_daily.csv')
patients=pd.read_csv('C:/Users/Afifa/Desktop/IHHPET notes/Covid projrct/corona virus pandemic/state_wise_daily.csv')
total=patients.shape[0]
active=patients[patients['Status']=='Confirmed'].shape[0]
recovered=patients[patients['Status']=='Recovered'].shape[0]
deceased=patients[patients['Status']=='Deceased'].shape[0]

options1=[
    {'label':'All','value':'All'},
    {'label':'Hospitalized','value':'Hospitalized'},
    {'label':'Recovered','value':'Recovered'},
    {'label':'Deceased','value':'Deceased'}
    ]

options2=[
    {'label':'All','value':'All'},
    {'label':'Mask','value':'Mask'},
    {'label':'Sanitizer','value':'Sanitizer'},
    {'label':'Oxygen','value':'Oxygen'},
    ]

options3=[
      {'label':'Red Zone', 'value':'Red Zone'},     
      {'label':'Blue Zone','value':'Blue Zone'},     
      {'label':'Green Zone','value':'Green Zone'},     
      {'label':'Orange Zone','value':'Orange Zone'} 
    ]
 
 ### to host the website in local server
app = dash.Dash(__name__,external_stylesheets=external_stylesheet) 

### to make app layout: container class
app.layout=html.Div([
        html.H1('Corona Virus Pandamic',style={'color':'#33FFD4 ', 'text-align':'center'}),
        
        ### Row 1- 4 columns(cards)
        html.Div([
            html.Div([
                html.Div([
                    html.Div([
                        html.H3("Total Cases",className='text-light'),
                        html.H4(total,className='text-light')
                        ],className='card-body')
                    ],className='card bg-danger')
                ],className='col-md-3'),
            
            
            html.Div([
                html.Div([
                    html.Div([
                        html.H3("Active Cases",className='text-light'),
                        html.H4(active,className='text-light')
                        ],className='card-body')    #card-body
                    ],className='card bg-info')     #card with bg colour
                ],className='col-md-3'),
            
            
            html.Div([
                html.Div([
                    html.Div([
                        html.H3("Recovered Cases",className='text-light'),
                        html.H4(recovered,className='text-light')
                        ],className='card-body')         #card-body
                    ],className='card bg-warning')       #card with bg colour
                ],className='col-md-3'),
                      
            html.Div([
                html.Div([
                    html.Div([
                        html.H3("Total Deaths",className='text-light'),
                        html.H4(deceased,className='text-light')
                        ],className='card-body')
                    ],className='card bg-success')
                ],className='col-md-3'),
            ],className='row'),
        
        ### Row 2
        html.Div([
            html.Div([
                html.Div([
                    html.Div([
                        dcc.Dropdown(id='plot-graph', options=options2, value='All'),
                        dcc.Graph(id='graph')
                        ],className='card-body')
                    ],className='card bg-success')
                ],className='col-md-6'),
            
            
            html.Div([
                html.Div([
                    html.Div([
                        dcc.Dropdown(id='my_dropdown', options=options3, value='Status'),
                        dcc.Graph(id='the_graph')
                        ],className='card-body')
                    ],className='card bg-info')
                ],className='col-md-6'),
            
            ],className='row'),
    
        
        ### Row 3
        html.Div([
            html.Div([
                html.Div([
                    html.Div([
                        # for graphs we use dcc(dash core components)
                        dcc.Dropdown(id='picker',options=options1, value='All'),
                        dcc.Graph(id='bar')
                        ],className='card-body')
                    ],className='card bg-warning')
                ],className='col-md-12')
            ],className='row'),
        
    ],className='container',style={'background-color': '#000000 '})

### for row 3
@app.callback(Output('bar','figure'),[Input('picker','value')])
def update_graph(type):
    if type=='All':
        return {'data':[go.Bar(x=patients['State'],y=patients['Total'])],
                'layout':go.Layout(title='State Total Count',plot_bgcolor='orange')
                }
        
    if type=='Hospitalized':
        return {'data':[go.Bar(x=patients['State'],y=patients['Hospitalized'])],
                'layout':go.Layout(title='State Total Count',plot_bgcolor='orange')
                }
        
    if type=='Recovered':
        return {'data':[go.Bar(x=patients['State'],y=patients['Recovered'])],
                'layout':go.Layout(title='State Total Count',plot_bgcolor='orange')
                }
        
    if type=='Deceased':
        return {'data':[go.Bar(x=patients['State'],y=patients['Deceased'])],
                'layout':go.Layout(title='State Total Count',plot_bgcolor='orange')
                }

### For Row 2(1)
@app.callback(Output('graph','figure'),[Input('plot-graph','value')])
def generate_graph(type):
    if type=='All':
        return {'data':[go.Line(x=patients['Status'], y=patients['Total'])], 
                'layout':go.Layout(title= "Commodities Total Count",plot_bgcolor='pink')} 
        
    if type=='Mask':
        return {'data':[go.Line(x=patients['Status'],  y=patients['Mask'])], 
                'layout':go.Layout(title= "Commodities Total Count",plot_bgcolor='pink')} 
        
    if type=='Sanitizer':
        return {'data':[go.Line(x=patients['Status'],  y=patients['Sanitizer'])], 
                'layout':go.Layout(title= "Commodities Total Count",plot_bgcolor='pink')} 
        
    if type=='Oxygen':
        return {'data':[go.Line(x=patients['Status'], y=patients['Oxygen'])], 
                'layout':go.Layout(title= "Commodities Total Count",plot_bgcolor='pink')} 
          
### For row 2(2)
@app.callback(Output('the_graph','figure'),[Input('my_dropdown','value')])
def generate_graph(my_dropdown):
    piechart = px.pie(data_frame=patients,names= my_dropdown, hole=0.3) 
    return (piechart)

if __name__ == '__main__': 
    # to get a local host from the class
    app.run_server(debug = True) 

   ##The Dashboard
![image](https://github.com/Faiqua-github/Covid-19-Impact-Analysis/assets/142328196/fb15c4d2-79ba-4674-b64b-3167eec3c22e)

## Comodities Options
![image](https://github.com/Faiqua-github/Covid-19-Impact-Analysis/assets/142328196/83819296-f4ff-4189-bdba-75d94f67106d)

##Zones Options
![image](https://github.com/Faiqua-github/Covid-19-Impact-Analysis/assets/142328196/f6cab73e-5bef-4874-bbfc-6df4c5c65e1b)

##Hospitalized, Deceased, Recovered Option in all states
![image](https://github.com/Faiqua-github/Covid-19-Impact-Analysis/assets/142328196/827f16d1-5736-4d73-a1fe-2777ad349023)

