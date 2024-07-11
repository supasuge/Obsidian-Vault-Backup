## Table of Contents


#httpmethods 
#GET
1. **HTTP GET**   
This retrieves a resource from the server. It is idempotent. Multiple identical requests return the same result.   
#PUT
2. **HTTP PUT**   
This updates or Creates a resource. It is idempotent. Multiple identical requests will update the same resource.   
#POST   
3. **HTTP POST**   
This is used to create new resources. It is not idempotent, making two identical POST will duplicate the resource creation.   
#DELETE
4. **HTTP DELETE**   
This is used to delete a resource. It is idempotent. Multiple identical requests will delete the same resource.   
#PATCH
5. **HTTP PATCH**   
The PATCH method applies partial modifications to a resource.   
#HEAD
6. **HTTP HEAD**   
The HEAD method asks for a response identical to a GET request but without the response body.   
#CONNECT
7. **HTTP CONNECT**   
The CONNECT method establishes a tunnel to the server identified by the target resource.   
#OPTIONS
8. **HTTP OPTIONS**   
This describes the communication options for the target resource.   
#TRACE
9. **HTTP TRACE**   
This performs a message loop-back test along the path to the target resource.
---
 **Headers**: Metadata fields that carry vital information about the request or response.
- **Paths**: The specific locations or resources you're aiming to access.
- **Arguments**: Data points that can alter or dictate the behavior of your request.
- **Form Data**: Data transferred from web forms.
- **JSON**: A popular data interchange format that's lightweight and human-readable.
- **Cookies**: Small data fragments stored on the user's computer, crucial for session management and tracking.
- **Redirects**: Methods web services use to direct your browser from one location to anothe
---
![[Pasted image 20231123083338.png]]