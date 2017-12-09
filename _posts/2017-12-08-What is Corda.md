<h2>The Corda Network</h2>

Corda is a fully peer to peer distributed ledger system.
However, it is different to a blockchain network in that it does not use a broadcast system where all transactions are sent to all nodes, transactions are between specified parties, like email.

<h3>States</h3>

States in corda can represent anything, known as sharded facts on a codra ledger (sql database).  States (aka shared facts) cannot change, we update by creating a new state.  Only the current state is considered on-ledger.  States are statically typed, eg an IOUState is always an IOUState.  States should model the real world instrument they are trying to reflect, eg bonds - bond state, cash - cash state, iou - iou state, demands - demand state etc.

State Sequence - is the sequence of all state events from inception up to the current state head.  The state head is saved in the vault.
The Ledger is the collection of all state heads.

States implement contract state, eg: 
//IOUs that we issue onto a ledger will simply be instances of this class.
public class IOUState implements ContractState {
    private final int value;
    private final Party lender;
    private final Party borrower;

    public IOUState(int value, Party lender, Party borrower) {
        this.value = value;
        this.lender = lender;
        this.borrower = borrower;
    }....
}

<h3>Transactions</h3>

Transactions are proposals to update the ledger, only committed if
no double spend - can use validating or simple notary
verified by all parties - digital signature
pass all contract constraints and responder constraints

There are three types of transaction, issuance, update and exit.

Can be many state objects in a transaction, eg paying a cash state for a ownership of a bond state.
Transactions can split states.  eg one cash input state of 10 quid can be split into 2 cash output states of 5 quid each.

All state objects in a transaction must occour or none occur.
Transactions are complex and need to be handled by flows.
Flows are at the highest level, and how we interact with nodes, either via the corda crash shell or by a custom writted web api using corda's rpc. eg on corda node using crash shell run - start IOUFlow arg0: 99, arg1: "O=PartyB,L=New York,C=US"


public class IOUFlow extends FlowLogic<Void> {
    private final Integer iouValue;
    private final Party otherParty;

    //The progress tracker provides checkpoints indicating the progress of the flow to observers.
    private final ProgressTracker progressTracker = new ProgressTracker();

    public IOUFlow(Integer iouValue, Party otherParty) {
        this.iouValue = iouValue;
        this.otherParty = otherParty;
    }....

    final TransactionBuilder txBuilder = new TransactionBuilder(notary);
    txBuilder.withItems(outputContractAndState, cmd);
    txBuilder.verify(getServiceHub()); - does this run the contract?
    final SignedTransaction signedTx = getServiceHub().signInitialTransaction(txBuilder);
    // Creating a session with the borrower to request their signature over the transaction
    //If the counterparty (otherParty) has a FlowLogic registered to respond to the FlowLogic initiating the session, a session will be     established
    FlowSession otherpartySession = initiateFlow(otherParty);

    // Obtaining the counterparty's signature.  once we have a flowsession established, we get the borrowers sig using          CollectSignaturesFlow
    //subFlow takes a fully signed transaction and a list of flow-sessions between the flow initiator and the required signers
    //returns a transaction signed by all the required signers
    //this initiates IOUFow responder!
    SignedTransaction fullySignedTx = subFlow(new CollectSignaturesFlow(
            signedTx, ImmutableList.of(otherpartySession), CollectSignaturesFlow.tracker()));

    // we pass the fully signed transaction (fullySignedTx) into FinalityFlow
    subFlow(new FinalityFlow(fullySignedTx));
.......
}


<h3>Contracts</h3> - code and legal prose

Contract code has one function verify, that takes one argument, the transaction.
The verify function defines constraints that if met throw an exception and the transaction will not be committed to the ledger.
legal prose is the legal wording that can accompany a code contract, that can be used in any disputes.

<h3>Commands</h3>

Commands are made an action, like issue/transfer/pay/redeem etc, plus a list of required signers.

<h3>Flows</h3>

Can be fully automated or need manual intervention?
Each flow needs a checkpoint manager, to save how far a transaction has progressed.  if a node fails and is restarted, checkpointed nodes can restart and pick up from a checkpoint - uses qusar to do this.

Flow library - corda reuseable methods that allow you to complete complicated parts of transactions, instead of devlopers trying to write them.

<h3>Notary</h3>

if there are no input states a notary service is not needed.
There are 2 tyes of notary
non-validating aka simple notary, this checks all the input states of a transaction to verify they have not already been spent.  The notary holds a list of all state hashes, if the new input state is hash is already on the list then the notary can not validate as it has already been spent.
validation notary - does the same check as the simple notary plus checks for double spending the same way as bitcoin does, by using the unspent transaction output model, this infers that money is in the system from, for example a cash issuance by a central bank.  The issue with this system of notary in corda is that transactions are private but to check if money has already been spent we need to go through all notarys on the system and map transactions back to the inception of the cash, which is not private, data is then shared.

multiple notarys can exist on the network.

<h3>Oracles</h3>

Network services that upon request provide information, examples are interest rate oracle, current value of option oracle etc

<h3>Nodes</h3>
Corda nodes run within JVM
Node storage and vault is SQL database, H2 in memory? or write to file system.
Attachments saved in a nodes local file system.

RPC client framework is used to communiate with a corda node.  We can use the rpc calls by calling a web api which uses the rpc to call a node.
You can also use crash shell on a node to commmunicate directly with a corda node.



I also dont know how markdown works:

http://packetlife.net/media/library/16/Markdown.pdf
