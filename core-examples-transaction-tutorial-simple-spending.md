Dash Core provides several RPCs which handle all the details of spending, including creating a <<glossary:change output>> and paying an appropriate <<glossary:transaction fee>>. Even advanced users should use these RPCs whenever possible to decrease the chance that <<glossary:duffs>> will be lost by mistake.

# 1. Get new address

Get a new Dash <<glossary:address>> and save it in the shell variable `$NEW_ADDRESS`.

``` bash
> dash-cli -regtest getnewaddress
yLp6ZJueuigiF4s9E1Pv8tEunDPEsjyQfd

> NEW_ADDRESS=yLp6ZJueuigiF4s9E1Pv8tEunDPEsjyQfd
```

#2. Send to address

Send 10 dash to the address using the [`sendtoaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-sendtoaddress).  The returned hex string is the transaction identifier (<<glossary:TXID>>).

The [`sendtoaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-sendtoaddress) automatically selects an <<glossary:unspent transaction output>> (UTXO) from which to spend the duffs. In this case, it withdrew the duffs from our only available UTXO, the <<glossary:coinbase transaction>> for <<glossary:block>> #1 which matured with the creation of block #101. To spend a specific UTXO, you could use the [`sendfrom` RPC](core-api-ref-remote-procedure-calls-wallet#section-sendfrom) instead.

``` bash
> dash-cli -regtest sendtoaddress $NEW_ADDRESS 10.00
c7e5ae1240fdd83bb94c94a93816ed2ab7bcb56ec3ff8a9725c5c1e0482684ea
```

# 3. List unspent outputs

# 3a. Confirmed outputs only

Use the [`listunspent` RPC](core-api-ref-remote-procedure-calls-wallet#section-listunspent) to display the UTXOs belonging to this <<glossary:wallet>>. The list is empty because it defaults to only showing confirmed UTXOs and we just spent our only confirmed UTXO.

``` bash
> dash-cli -regtest listunspent
[
]
```

# 3b. All outputs

Re-running the [`listunspent` RPC](core-api-ref-remote-procedure-calls-wallet#section-listunspent) with the argument "0" to also display each <<glossary:unconfirmed transaction>> shows that we have two UTXOs, both with the same <<glossary:TXID>>. The first UTXO shown is a change output that `sendtoaddress` created using a new address from the key pool. The second UTXO shown is the spend to the address we provided. If we had spent those duffs to someone else, that second transaction would not be displayed in our list of UTXOs.

``` bash
> dash-cli -regtest listunspent 0
```

``` json
[  
   {  
      "txid":"c7e5ae1240fdd83bb94c94a93816ed2ab7bcb56ec3ff8a9725c5c1e0482684ea",
      "vout":0,
      "address":"yLp6ZJueuigiF4s9E1Pv8tEunDPEsjyQfd",
      "account":"",
      "scriptPubKey":"76a914056b1fe57914236149feb21dcbc6b86f4bdd9f4988ac",
      "amount":10.00000000,
      "confirmations":0,
      "ps_rounds":-2,
      "spendable":true,
      "solvable":true
   },
   {  
      "txid":"c7e5ae1240fdd83bb94c94a93816ed2ab7bcb56ec3ff8a9725c5c1e0482684ea",
      "vout":1,
      "address":"yeP6Tw2uW4nWAFWRytw8TyshErTq59dUkN",
      "scriptPubKey":"76a914c622e98a6ccf34d02620612f58f20a50061cf4b188ac",
      "amount":490.00000000,
      "confirmations":0,
      "ps_rounds":-2,
      "spendable":true,
      "solvable":true
   }
]
```

# 4. Mine block

Create a new block to confirm the transaction above (takes less than a second) and clear the shell variable.

``` bash
> dash-cli -regtest generate 1

> unset NEW_ADDRESS
```

# Simple Spending Script

Shell script to run the previous example (available [here](https://gist.github.com/dash-docs/f40bddfc0844ec0d66d196720dc936f8#file-regtest_transaction_simple_spending_example-sh)):

[block:code]
{
  "codes": [
    {
      "code": "#!/bin/bash\n\n# Set RPCUSER and RPCPWD to the username and password configure in dash.conf\nRPCUSER=\"user\"\nRPCPWD=\"pass\"\nREGTEST_CMD=\"dash-cli -regtest -rpcuser=$RPCUSER -rpcpassword=$RPCPWD\"\n\n# SIMPLE SPENDING EXAMPLE\n# -----------------------\n\nBLOCKTOGEN=101\nprintf \"\\nGenerating %s block(s)...\\n\" $BLOCKTOGEN\nCOMMAND=\"$REGTEST_CMD generate $BLOCKTOGEN\"\necho $COMMAND\nGENERATED=$($COMMAND)\necho \"Generated $BLOCKTOGEN block(s)\"\n\nprintf \"\\nChecking balance...\\n\"\nCOMMAND=\"$REGTEST_CMD getbalance\"\necho $COMMAND\nBALANCE=$($COMMAND)\necho \"Balance is: \" $BALANCE\n\nprintf \"\\nGetting new address...\\n\"\nCOMMAND=\"$REGTEST_CMD getnewaddress\"\necho $COMMAND\nNEW_ADDRESS=$($COMMAND)\necho \"New address is: \" $NEW_ADDRESS\n\nprintf \"\\nSending to address...\\n\"\nCOMMAND=\"$REGTEST_CMD sendtoaddress $NEW_ADDRESS 10.00\"\necho $COMMAND\nTXID=$($COMMAND)\necho \"Transaction ID (TXID) is: \" $TXID\n\nprintf \"\\nList unspent...\\n\"\nCOMMAND=\"$REGTEST_CMD listunspent\"\necho $COMMAND\nUNSPENT=$($COMMAND)\necho \"Unspent (excluding unconfirmed): \" $UNSPENT\n\nprintf \"\\nList unspent (unconfirmed)...\\n\"\nCOMMAND=\"$REGTEST_CMD listunspent 0\"\necho $COMMAND\nUNSPENT=$($COMMAND)\necho \"Unspent (including unconfirmed): \" $UNSPENT\n\n\nBLOCKTOGEN=1\nprintf \"\\nGenerating %s block(s) to confirm...\\n\" $BLOCKTOGEN\nCOMMAND=\"$REGTEST_CMD generate $BLOCKTOGEN\"\necho $COMMAND\nGENERATED=$($COMMAND)\necho \"Generated $BLOCKTOGEN block(s)\"\n\nprintf \"\\nList unspent...\\n\"\nCOMMAND=\"$REGTEST_CMD listunspent\"\necho $COMMAND\nUNSPENT=$($COMMAND)\necho \"Unspent (excluding unconfirmed): \" $UNSPENT\n\nunset REGTEST_CMD\nunset BLOCKTOGEN\nunset COMMAND\nunset GENERATED\nunset BALANCE\nunset NEW_ADDRESS\nunset TXID\nunset UNSPENT",
      "language": "shell",
      "name": "Shell"
    }
  ]
}
[/block]