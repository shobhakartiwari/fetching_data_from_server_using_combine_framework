# fetching_data_from_server_using_combine_framework
Combine framework to fetch data from server - example of RFP ( reactive functional programming )

```
import Combine
struct UserModel: Decodable {
    let title : String?
    let body  : String?
}

class ApiManager {
    
    
    func getDataFromServerFor(url : String = "https://jsonplaceholder.typicode.com/posts") -> AnyPublisher<[UserModel], Error> {
        
        guard let serverUrl = URL(string: url) else {
            fatalError("server url error")
            
        }
        
        return URLSession.shared.dataTaskPublisher(for: serverUrl)
            .map{$0.data}
            .decode(type: [UserModel].self, decoder: JSONDecoder())
            .eraseToAnyPublisher()
                        
    }
    
}



//fetch data from server
let publisher = ApiManager().getDataFromServerFor()
//add subscriber
let cancellable = publisher.sink { _ in
                    //TODO code
                } receiveValue: { receviedPost in
                    print(receviedPost)
                }
                
                
                '''
