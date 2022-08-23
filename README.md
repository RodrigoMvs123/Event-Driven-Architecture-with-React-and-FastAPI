# Event-Driven-Architecture-with-React-and-FastAPI

Event-Driven Architecture with React and FastAPI
Event-Driven Architecture with React and FastAPI – Full Course

https://fastapi.tiangolo.com/

https://redis.com/
https://app.redislabs.com/#/subscriptions/subscription/1829476/bdb
pip install redis_om 

Public end-point “redis-14471.c16.us-east-1-3.ec2.cloud.redislabs.com:14471
port:”02sCUi6HLYIcKDYvPHhb39coacu2p9u0”‘

https://www.postman.com/
https://web.postman.co/workspace/My-Workspace~cce2ca3f-45cf-4dbd-8beb-c6fb4f264aea/request/22945770-ba04d884-6225-4eb6-843d-f91c0eec6763

https://getbootstrap.com/


Main.py 
from json import loads
from urllib.request import Request
from fastapi import FastAPI, Request
from fastapi.middleware.cors import CORSmiddleware
from pyparsing import re
from redis_om import get_redis_connection, HashModel

app = FastAPI()

app.add_middleware(
    CORSmiddleware,
    allow_origins=['http://localhost:3000'],
    allow_methods=['*'],
    allow_headers=['*']
)

#pip install redis_om

redis = get_redis_connection(
    host="redis-14471.c16.us-east-1-3.ec2.cloud.redislabs.com",
    port=14471,
    password="02sCUi6HLYIcKDYvPHhb39coacu2p9u0",
    decode_responses=True
)

#uvicorn main:app --reload

class Delivery(HashModel):
    budget: int = 0 
    notes: str= '' 

    class Meta: 
        database = redis 

class Event(HashModel):
  delivery_id: str = None
    type: str
    data: str 

    class Meta: 
        database = redis 

    @app.get('/deliveries/{pk}/status')
    async def get_state(pk: str):
         state = redis.get(f'delivery:{pk}')

         if state is not None: 
            return.json.loads(state)

            state = build_state(pk)
             redis.set(f'delivery:{pk}', json.dumps(state))
            return state     

        def build_state(pk: str):
            pks = Event.all.pks()
            all_events = [Event_get(pk) for pk in pks]
            events = [event for event in all_events if event.delivery_id == pk]
            state = {}
['01G86J7K0K979F8D3C03AT39ZH', '01G86J7REYNJ6RVFF1BJD221PF', '01G86JYWAE0SCYSZ0AZDO8WGW', '01G86J840PQJT0R142QEY32WZ8']
            for event in events:
                state = consumers.CONSUMERS[event.type](state, event)


            return state 

# uvicorn main:app -- reload

    @app.post('/deliveries/create')
    async def create(request:Request):
        body = await request.json()
        delivery = Delivery(budget=body['data']['budget'], notes=body['data']['notes']).save()
        event = Event(delivery_id=delivery.pk, type=body['type'], data=json.dumps(body['data'])).save()
        state = consumers.CONSUMERS[event.type]({}, event)
        redis.set(f'delivery:{delivery.pk}', json.dumps(state))
        return state

        #https://web.postman.co/workspace/My-Workspace~cce2ca3f-45cf-4dbd-8beb-c6fb4f264aea/request/22945770-ba04d884-6225-4eb6-843d-f91c0eec6763

    @app.post('/event')
    async def dispatch(request:Request):
         body = await request.json()
         delivery_id = body['delivery_id']
         event = Event(delivery_id=delivery_id, type=body['type'], data=json.dumps(body['data'])).save()
         state = await get_state(delivery_id)
         new_state = consumers.CONSUMERS[event.type](state, event)
         redis.set(f'delivery:{delivery_id}', json.dumps(new_state))
         return new_state



         #Redis Clound 
         #http://localhost:8000/event
         #Body
         #{
        #  type: "Start Delivery",
        #  "delivery_id": "01G85YDMG4M6FTW19N688KMC3"
         # }




Prompt
Visual Stutio Code
npm start 
npm i bootstrap


app.js
import 'bootstrap/dist/css/bootstrap.css'

function app() {
    const [id, setId] = useState('initialState:'') 
    
    const submit = async (e) => {
        e.preventDefault();
        const form = new FormData(e.target);
        const = data = Object.fromEntries(from.entries());
        const response = await fetch(input:'http://localhost:8000/deliveries/create', init:{
            method: 'POST',
            headers: {'Content-Type':'application/json'}, 
            body: JSON.stringfy(value:{
                type:"CREATE_DELIVERY",
                data
            });

        const {id} = await response.json();
        setId(id)
    }


    <div className="col-3">
           <div className="card">
            {id === '' ? <div className="card">
                 <div className="card-header">
                     Start Delivery
                 </div>
                 <form className="card-body" onSumit={e => submit(e, type: "START_DELIVERY")}>
                    <button className="btn btn-primary">Submit</button>
                </form>
            </div>
        </div>        

    <div className="col-3">
        <div className="card">
         {id === '' ? <div className="card">
              <div className="card-header">
                  Increase Budget 
              </div>
              <form className="card-body" onSumit={e => submit(e, type: "INCREASE_BUDGET")}>
                  <div className="input-group mb-3">
                <input type="number" name="budget" className="form-control" placeholder="Budget"/>  
                </div>
                 <button className="btn btn-primary">Submit</button>

             </form>
         </div>
     </div>   
     
     
     <div className="col-3">
     <div className="card">
      {id === '' ? <div className="card">
           <div className="card-header">
               Pickup Products 
           </div>
           <form className="card-body" onSumit={e => submit(e, type: "PICKUP_PRODUCTS")}>
               <div className="input-group mb-3">
             <input type="number" name="purchase price" className="form-control" 
                    placeholder="Purchase Price"/>  
             <input type="number" name="quantity" className="form-control" placeholder="Quantity"/> 
             </div>
              <button className="btn btn-primary">Submit</button>

          </form>
      </div>
  </div>   
  
  <div className="col-3">
     <div className="card">
      {id === '' ? <div className="card">
           <div className="card-header">
              Deliver Products 
           </div>
           <form className="card-body" onSumit={e => submit(e, type: "DELIVER_PRODUCTS")} >
               <div className="input-group mb-3">
             <input type="number" name="sell_price" className="form-control" 
                    placeholder="Sell Price"/>  
             <input type="number" name="quantity" className="form-control" placeholder="Quantity"/> 
             </div>
              <button className="btn btn-primary">Submit</button>

          </form>
      </div> : <Delivery id{id}/>}
        </div>
        <code className="col-12 mt-4">
          {JSON.stringify(state)}
        </code>
    </div>
}

    export default App;
    
    
    Delivery.js
    import React from 'react';
const Delivery = () => {
        const [state, setState] = useState(initialState{});
        const [refresh, setRefresh] = useState (initialState: False);

 
        useEffect((effect:() => {
            (async () =>{
                const response = await fetch (input: 'http://localhost:8000/deliveries/${props.id}/status');
                const data = await response.json();
                setState(data);
            }) ()
        }, deps:[refresh]);

         const submit = async (e, type) => {
             e.preventDefault();
             const form = new FormData(e.target);
             const = data = Object.fromEntries(from.entries());
             const response = await fetch(input:'http://localhost:8000/event', init:{
                 method: 'POST',
                 headers: {'Content-Type':'application/json'}, 
                 body: JSON.stringfy(value:{
                     type,
                     data,
                     delivery_id: state.id
                 });
             
                 if (!response.ok){
                     const {detail} = await response.json();
                     alert (detail);
                     return;
                 }
                 setRefresh(!refresh);
         }

        return <div className="row w-100">
            <div className="col-12 mb-4">
                <h4 className="fw-bold text-white">Delivery {state.id}</h4>

            </div>
            <div className="col-12 mb-5">
                <div className="progress">
                   { state.status !== 'ready' ? 
                   <div className= {state.status === 'active' ?  "progress-bar bg-success progress-bar-striped progress-bar-animated":  "progress-bar bg-success " } 
                          role="progressbar" style={{width: '50%'}}
                        ></div> ''}
                        {state.status === 'collected' || state.status === 'completed' ?
                        <div"className= {state.status === 'collected' ?  "progress-bar bg-success progress-bar-striped progress-bar-animated":  "progress-bar bg-success " } "  
                        role="progressbar" style={{width: '50%'}}
                        ></div> : '' }
                     </div>
                </div>
                <div className="col-3">
                    
                </div>

            </div>
};

export defaut Delivery;


Index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pizzas</title>
</head>
<body class="bg-dark text-secondary px-4 py-5 text-center"> 
    
</body>
</html>


Consumers.py
from http.client import HTTPException


def create_delivery(state, event):
    data = json.loads(event.data)
    return{
        "id": event.delivery_id,
        "budget": int(data["budget"]),
        "notes": data["notes"],
        "status": "ready"
    }

    def start_delivery(state, event):
        if state ['status'] != 'ready':
            raise HTTPException(status_code=400, detail="Delivery already started")
        
        return state | {
            "status": "active"
        }

    def pickup_products(state, event):
         data = json.loads(event.data)
         new_budget = state["budget"] - int (data ['purchase_price']) * int (data ['quantity'])

         if new_budget < 0: 
            raise HTTPException(status_code=400, detail="Not enough budget")

         return state | {
             "budget": new_budget,
             "purchase_price": int (data ['purchase_price']),
             "quantity": int (data ['quantity']),
             "status": "collected"
         }

    def deliver_products(state, event):
         data = json.loads(event.data)
         new_budget = state["budget"] + int (data ['sell_price']) * int (data ['quantity'])
         new_quantity = state["quantity"] - int (data ['quantity'])
        
        if new_quantity < 0: 
            raise HTTPException(status_code=400, detail="Not enough quantity")

         return state | {
             "budget": new_budget,
             "sell_price": int (data ['sell_price']),
             "quantity": new_quantity,
             "status": "completed" 
         }

    def increase_budget(state, event):
        data = json.loads(event.data)
        state['budget'] += int(data['budget'])
        return state


        CONSUMERS = {
            "CREATE_DELIVERY": create_delivery,
            "START_DELIVERY": start_delivery,
            "PICKUP_PRODUCTS": pickup_products,
            "DELIVER_PRODUCTS": deliver_products,
            "INCREASE_BUDGET": increase_budget,
        }
        
        
        
