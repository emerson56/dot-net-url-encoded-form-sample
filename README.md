# dot-net-url-encoded-form-sample
// Boiler plate for making api call using httpclient

    public static async Task<Object> GetResult(SomeRequest request)
    {
          
            string url = Constants.Url;

            var requestMessage = new HttpRequestMessage(GetMethod(request.method), url);
            
            // add headers
            foreach(var header in request.headers)
            {
                requestMessage.Headers.TryAddWithoutValidation (header.Key, header.Value);
            }

            if(!string.IsNullOrWhiteSpace(request.body))
            {
                var kvps = request.body.Split('&')
                    .Select(x => x.Split('='))
                    .ToDictionary(x => x[0], x => x[1]);

                requestMessage.Content = new FormUrlEncodedContent(kvps);
            }

            var rawResponse = await client.SendAsync(requestMessage);
            
            //if (rawResponse.IsSuccessStatusCode)
            //{
                //string rawContent = await rawResponse.Content.ReadAsStringAsync();
                //response = JsonConvert.DeserializeObject<Object>(rawContent);
            //}

            return rawResponse;
    }
