# test-task-for-betterme

**TC01** - Successful pet update
**Precondition:** A pet has been previously created using `POST /pet`

**Steps:**
1. Retrieve and store the `id` od the created pet
2. Send a `PUT` requst with updated fields:
    - New `name` (e.g. `Fluffy_Update`)
    - New `status` (e.g. `sold`)
    - Tags (e.g. `[{"id": "99", "name": "unknownHamster"}]`)

**Expected Result:**
- Satus code `200 OK`
- The response contains updated data for `name`, `status` and `tags`
- The `id` remains unchanged

**TC02** - Updated without the `status` field

**Steps:**
1. Prepare a `PUT /pet` request body that omits the `status` field
2. Include the other required fields: `id`, `name`, `tags`, `photoUrls`
3. Send the request

**Expected Result:**
- Api returns `200 OK`
- When using a `GET` request, status is missing from the response

**TC03** - Updated with an empty `tags` array

**Steps:**
1. Prepare a `PUT /pet` request where `"tags": []`
2. Keep all other required fields (`id`, `name`, `status`, `photoUrls`) valid
3. Send the request
4. Perform a `GET /pet/{id}` to verify the result

**Expected Result:**
- Tags are removed from the pet
- Api returns `200 OK`

**TC04** - Replace tags with `"unknownHamster"`

**Steps:**
1. In the `PUT /pet` request body, set the `tags` field as follows:
   <pre> json "tags": [ { "id": 99, "name": "unknownHamster" } ] </pre>
2. Send the request
3. Perform a `GET /pet/{id}` to verify the updated tags

**Expected Result:**
- The response contains exactly one tag
- The tag has `id = 99` and `name = "unknownHamster"`
- Status code is `200 OK`

**TC05** - Updated a non-exisping pet

**Steps:**
1.  Prepare a `PUT /pet` request with a pen `id` that does not exist in the system (e.g. `99999`)
2.  Fill in the other required fields: `name`, `status`, `tags`, etc.
3.  Send the request

**Expected Result:**
- Status code is `200 OK`
- A new pet is created with the provided `id` and data

**TC06** - Update with invalid `tag` format (missing id)

**Steps:**
1. Prepare a `PUT /pet` request where the `tags` array contains an object without the `id` field, for example:
   <pre> json "tags": [ { "name": "brokenTag" } ] </pre>
2. Include other required fields: `id`, `name`, `status`, etc.
3.  Send the request

**Expected Result:**
- Status code is `200 OK`
- The returned `tags` array contains a single tag with:
    - `name = "brokenTag"`
    - `id = 0` (automatically assigned by the system)
 
**TC07** - Update with special characters in the name 

**Steps:**
1. Prepare a `PUT /pet` request with a `name` that includes special characters, e.g.: `"name": "Tyler_#@!*&"`
2. Ensure all other required fields are valid
3. Send the request
4. Retrieve the pet with `GET /pet/{id}`

**Expected Result:**
- Status code is `200 OK`
- The special characters are preserved in the response

**TC08** -  Update with a very long `name` (300+ characters)

**Steps:**
1. Generate a string with more than 300 characters (e.g., `"A"` repeated 300 times)
2. Set this string as the value of the `name` field in the `PUT /pet` request
3. Ensure all other required fields (`id`, `status`, etc.) are valid
4. Send the request
   
**Expected Result:**
- Status code is `200 OK`
- The long `name` is saved and returned in the response as provided

**TC09** - Update with an empty request body

**Steps:**
1. Send a `PUT /pet` request with an empty JSON body: `{}`
2. Send the request
3. Observe the response

**Expected Result:**
- Status code is `200 OK`
- The response body contains default values:
  <pre> json { "id": 9223372036854775807, "photoUrls": [], "tags": [] } </pre>

**TC10** - Update with a name that already exists for another pet

**Steps:**
1. Create two pets with different `id` values using `POST /pet`
2. Update the second pet via `PUT /pet`, setting its name to the same `name` as the first pet
3. Send the request

**Expected Result:**
- Status code is `200 OK`
- Duplicate names are allowed
- Both pets exist with the same `name`, but different `id`











