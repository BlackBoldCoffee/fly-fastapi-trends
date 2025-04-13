from fastapi import FastAPI, Query
from pytrends.request import TrendReq
from typing import List

app = FastAPI()

# Inițializează PyTrends setat pentru România
pytrends = TrendReq(hl='ro-RO', tz=360)

@app.get("/trends")
def get_trends(keywords: List[str] = Query(...)):
    """
    API care primește o listă de keyword-uri și întoarce datele de interes pe ultimele 3 luni.
    """
    try:
        pytrends.build_payload(keywords, cat=0, timeframe='today 3-m', geo='RO')
        data = pytrends.interest_over_time()
        if data.empty:
            return {"error": "Nu s-au obținut date pentru cuvintele cheie furnizate."}
        if 'isPartial' in data.columns:
            data = data.drop(columns=['isPartial'])
        return data.to_dict()
    except Exception as e:
        return {"error": str(e)}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
