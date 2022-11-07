# Domanda 1 come rendere meno stupido il tokenizer tramite chiamata ricorsiva di field

```cpp
bool Book::is_valid_isbn(std::string isbn){
    enum class String{starting_index = 0};

    //divido in campi l'isbn che è suddiviso tramite "-"
    //chiamando ogni volta la lambda function ottengo una coppia che rispettivamente
    //contiene il campo stesso sotto forma di stringa e la sottostringa rimanente
    //a destra del separatore
    auto field = [](std::string string, const std::string separator){
        auto position_end_field = string.find(separator);
        std::string field = string.substr(static_cast<int>(String::starting_index), position_end_field);
        //+1 perchè devo spostarmi con l'iteratore appena dopo il separatore "-"
        std::string remaining_string = string.substr(position_end_field+1);
        return std::make_pair(field, remaining_string);
    };

    //controllo ogni carattere della stringa, se trovo un elemento che non è
    //un numero ritorno false perchè in quel caso find_if_not non sarà arrivato a
    //string.end() altrimenti se la stringa è formata da soli numeri 
    auto is_digit = [](std::string &string){
        auto it = std::find_if_not(string.begin(), string.end(), [](char const &c){
            return isdigit(c);});
        return it == string.end();
    };

   auto is_alfanumeric = [](std::string &string)
    {
        auto it = std::find_if_not(string.begin(), string.end(), [](char const &c){
            return isalnum(c);});
        return it == string.end();
    };
    
    const std::string separator{"-"};
    //qua devo ottenere field(stringa, separatore).first
    //ho nestato field() in modo da inserire sempre la stringa di partenza
    //computazionalmente sto rifacendo calcoli che gia so?
    std::string first_numeric_field{field(isbn,separator).first};
    std::string second_numeric_field{field(field(isbn,separator).second, separator).first};
    std::string third_numeric_field{field(field(field(isbn, separator).second,separator).second, separator).first};
    std::string forth_alphanumeric_field{field(field(field(field(isbn, separator).second, separator).second,separator).second, separator).first};
    
    if(is_digit(first_numeric_field) && is_digit(second_numeric_field) && 
    is_digit(third_numeric_field) && is_alfanumeric(forth_alphanumeric_field))
    {
        return true;
    }


    return false;
}
```

# Domanda 2 Perchè devo mettere ::isdigit nel blocco di codice sotto (me lo espande a lambda function per avere una nuova execution  policy?)
```cpp
std::all_of(input.begin(), input.end(), ::isdigit) && input.size() != 0
```

# Domanda 3 Perchè l'overload di operatori ( diverso ad esempio da ostream) all' interno della classe accetta solo un argomento mentre all'esterno della classe ne accetta anche 2?

Ad esempio 

``` cpp
vec3& operator+=(const vec3 &v) {
            e[0] += v.e[0];
            e[1] += v.e[1];
            e[2] += v.e[2];
            return *this;
        }
```


invece se definisco `vec3& operator+=(const vec3 &v)` con due elementi mi ritorna questo errore `error: 'vec3& vec3::operator*=(double, int)' must have exactly one argumentx86-64 gcc 12.2 #1`
