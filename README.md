# giuappunti
public bool Add(int key, string value)
{
    var hashed = ComputeHashNumber(key);

    if (Elements[hashed].isFree == true)
    {
        Elements[hashed].value = value;
        Elements[hashed].key = key;
        Elements[hashed].isFree = false;
        return true;
    }
    else
    {
        // Linear probing: cerca la prossima posizione libera
        var index = hashed;
        
        while (true)
        {
            // Passa alla prossima posizione
            index = (index + 1) % _arrayCapacity;
            
            // Se torniamo alla posizione iniziale, la tabella è piena
            if (index == hashed)
            {
                return false; // Tabella piena
            }
            
            // Se troviamo una posizione libera
            if (Elements[index].isFree == true)
            {
                Elements[index].value = value;
                Elements[index].key = key;
                Elements[index].isFree = false;
                return true;
            }
        }
    }
}
public string Get(int key)
{
    var hashed = ComputeHashNumber(key);
    var index = hashed;
    
    while (true)
    {
        // Se troviamo una posizione libera, la chiave non esiste
        if (Elements[index].isFree == true)
        {
            return null; // Oppure throw new KeyNotFoundException()
        }
        
        // Se troviamo la chiave, restituisci il valore
        if (Elements[index].key == key)
        {
            return Elements[index].value;
        }
        
        // Passa alla prossima posizione (linear probing)
        index = (index + 1) % _arrayCapacity;
        
        // Se torniamo alla posizione iniziale, la chiave non esiste
        if (index == hashed)
        {
            return null;
        }
    }
}

// CONTAINS: verifica se la chiave esiste
public bool Contains(int key)
{
    var hashed = ComputeHashNumber(key);
    var index = hashed;
    
    while (true)
    {
        // Se troviamo una posizione libera, la chiave non esiste
        if (Elements[index].isFree == true)
        {
            return false;
        }
        
        // Se troviamo la chiave, esiste
        if (Elements[index].key == key)
        {
            return true;
        }
        
        // Passa alla prossima posizione
        index = (index + 1) % _arrayCapacity;
        
        // Se torniamo alla posizione iniziale, la chiave non esiste
        if (index == hashed)
        {
            return false;
        }
    }
}

// UPDATE: aggiorna il valore di una chiave esistente
public bool Update(int key, string newValue)
{
    var hashed = ComputeHashNumber(key);
    var index = hashed;
    
    while (true)
    {
        // Se troviamo una posizione libera, la chiave non esiste
        if (Elements[index].isFree == true)
        {
            return false; // Chiave non trovata
        }
        
        // Se troviamo la chiave, aggiorna il valore
        if (Elements[index].key == key)
        {
            Elements[index].value = newValue;
            return true; // Aggiornamento riuscito
        }
        
        // Passa alla prossima posizione
        index = (index + 1) % _arrayCapacity;
        
        // Se torniamo alla posizione iniziale, la chiave non esiste
        if (index == hashed)
        {
            return false;
        }
    }
}

// DELETE: elimina una chiave (lazy deletion)
public bool Delete(int key)
{
    var hashed = ComputeHashNumber(key);
    var index = hashed;
    
    while (true)
    {
        // Se troviamo una posizione libera, la chiave non esiste
        if (Elements[index].isFree == true)
        {
            return false; // Chiave non trovata
        }
        
        // Se troviamo la chiave, eliminala
        if (Elements[index].key == key)
        {
            Elements[index].isFree = true;
            Elements[index].value = null;
            Elements[index].key = 0; // Opzionale: reset della chiave
            return true; // Eliminazione riuscita
        }
        
        // Passa alla prossima posizione
        index = (index + 1) % _arrayCapacity;
        
        // Se torniamo alla posizione iniziale, la chiave non esiste
        if (index == hashed)
        {
            return false;
        }
    }
}