# StormGlass
Swift package of the Storm Glass API https://stormglass.io

Generated using the Create API https://github.com/CreateAPI/CreateAPI

## How to use it

    import Foundation
    import StormGlass
    import Get
    
    class NetworkService {
        private var client: APIClient
        private let url = URL(string: "https://api.marea.ooo/v2")
        
        init() {
            let configuration = APIClient.Configuration(baseURL: url, delegate: ClientDelegate())
            let client = APIClient(configuration: configuration)
            
            self.client = client
        }

        /// Example for fetching the wave height.
        /// You can fetch multiple weather parameters by spcifying them in the 'params', eg. "waveHeight,waveDirection,windSpeed'
        func waveForecastFor(latitude: Double, longitude: Double) async throws -> Paths.Point.GetResponse {
            let parameters = Paths.Point.GetParameters(lat: latitude, lng: longitude, params: "waveHeight")
            let request = Paths.point.get(parameters: parameters)
            let response = try await client.send(request)
            
            return response.value
        }
    }
    
    final class ClientDelegate: APIClientDelegate {
        private var accessToken: String = "replace-with-your-access-token"
        
        func client(_ client: APIClient, willSendRequest request: inout URLRequest) async throws {
            request.setValue("\(accessToken)", forHTTPHeaderField: "Authorization")
        }
        
        func client(_ client: APIClient, validateResponse response: HTTPURLResponse, data: Data, task: URLSessionTask) throws {
            
        }
    }
