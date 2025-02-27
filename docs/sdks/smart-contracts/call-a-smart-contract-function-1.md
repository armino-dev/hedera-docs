# Get a smart contract function

A query that calls a function of the given smart contract instance, giving it function parameters as its inputs. This is performed locally on the particular node that the client is communicating with. It cannot change the state of the contract instance (and so, cannot spend anything from the instance's cryptocurrency account). It will not have a consensus timestamp. It cannot generate a record or a receipt. The response will contain the output returned by the function call. This is useful for calling getter functions, which purely read the state and don't change it. It is faster and cheaper than a normal call, because it is purely local to a single node.

Unlike a contract execute transaction, the node will consume the entire amount of provided gas in determining the fee for this query.

**Query Signing Requirements**

* The client operator account's private key (fee payer) is required to sign this query

**Query Fees**

* Please see the transaction and query [fees](../../../mainnet/fees/#transaction-and-query-fees) table for base transaction fee
* Please use the [Hedera fee estimator](https://hedera.com/fees) to estimate your query fee cost

| Constructor               | Description                            |
| ------------------------- | -------------------------------------- |
| `new ContractCallQuery()` | Initializes a ContractCallQuery object |

```java
new ContractCallQuery()
```

### Methods

{% tabs %}
{% tab title="V2" %}
| Method                        | Type                                             | Description                                                                                                                             | Requirement          |
| ----------------------------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| `setContractId(<contractId>)` | [ContractId](../specialized-types.md#contractid) | Sets the contract instance to call, in the format used in transactions (x.z.y).                                                         | Required             |
| `setGas(<gas>)`               | long                                             | Sets the amount of gas to use for the call.                                                                                             | Required             |
| `setFunction(<name>)`         | String                                           | Sets the function name to call. The function will be called with no parameters.                                                         | Required             |
| `setFunction(<name, params>)` | <p>String, <br>ContractFunctionParameters</p>    | Sets the function to call, and the parameters to pass to the function.                                                                  | Optional             |
| `setMaxResultSize(<size>)`    | long                                             | Sets the max number of bytes that the result might include. The run will fail if it would have returned more than this number of bytes. | Deprecated \[0.20.0] |

{% code title="Java" %}
```java
//Contract call query
ContractCallQuery query = new ContractCallQuery()
    .setContractId(contractId)
    .setGas(600)
    .setFunction("greet"); 

//Sign with the client operator private key to pay for the query and submit the query to a Hedera network
ContractFunctionResult contractCallResult = query.execute(client);

// Get the function value
String message = contractCallResult.getString(0);
System.out.println("contract message: " + message);

//v2.0.0
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
//Contract call query
const query = new ContractCallQuery()
    .setContractId(contractId)
    .setGas(600)
    .setFunction("greet");

//Sign with the client operator private key to pay for the query and submit the query to a Hedera network
const contractCallResult = await query.execute(client);

// Get the function value
const message = contractCallResult.getString(0);
console.log("contract message: " + message);
```
{% endcode %}

{% code title="Go" %}
```java
//Contract call query
query := hedera.NewContractCallQuery().
		SetContractID(contractID).
		SetGas(600).
		SetFunction("greet", nil)

//Sign with the client operator private key to pay for the query and submit the query to a Hedera network
contractFunctionResult, err := query.Execute(client)

if err != nil {
		panic(err)
}

//Print the query results to the console
println(contractFunctionResult)
//v2.0.0
```
{% endcode %}

**Sample Output**



```
ContractFunctionResult{
     contractId=0.0.104984
     rawResult=000000000000000000000000000000000000000000000000000000000000002
        0000000000000000000000000000000000000000000000000000000000000000d48656c
        6c6f2c20776f726c642100000000000000000000000000000000000000, 
     bloom=, 
     gasUsed=581, 
     errorMessage=null, 
     logs=[]
}
```
{% endtab %}

{% tab title="V1" %}


| Method                        | Type                                              | Description                                                                                                                             | Requirement          |
| ----------------------------- | ------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| `setContractId(<contractId>)` | ContractId                                        | Sets the contract instance to call, in the format used in transactions (x.z.y).                                                         | Required             |
| `setGas(<gas>)`               | long                                              | Sets the amount of gas to use for the call.                                                                                             | Required             |
| `setFunction(<name>)`         | String                                            | Sets the function name to call. The function will be called with no parameters.                                                         | Required             |
| `setFunction(<name, params>)` | <p>String, <br>Contract<br>FunctionParameters</p> | Sets the function to call, and the parameters to pass to the function.                                                                  | Optional             |
| `setMaxResultSize(<size>)`    | long                                              | Sets the max number of bytes that the result might include. The run will fail if it would have returned more than this number of bytes. | Deprecated \[0.20.0] |

{% code title="Java" %}
```java
//Contract call query
ContractCallQuery query = new ContractCallQuery()
    .setContractId(newContractId)
    .setGas(600)
    .setFunction("greet");

//Sign with the client operator private key to pay for the query and submit the query to a Hedera network
ContractFunctionResult contractFunctionResult = query.execute(client);

System.out.println(contractFunctionResult);
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
//Contract call query
const query = new ContractCallQuery()
    .setContractId(newContractId)
    .setGas(600)
    .setFunction("greet");

//Sign with the client operator private key to pay for the query and submit the query to a Hedera network
const contractFunctionResult = await query.execute(client);

console.log(contractFunctionResult);
```
{% endcode %}
{% endtab %}
{% endtabs %}
