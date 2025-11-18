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