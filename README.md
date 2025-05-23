// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/extensions/ERC20URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract TokenizedBond is ERC20Compatable, Ownable {
    uint256 public BondPrice;
    uint256 public InterestRate;
    uint256 public IntervalRate;
    uint256 public PenaltyRate;
    uint256 public MaturityDate;
    string public BondRating;
    mapping(address =>bool) public WhiteListed;
    mapping(address =>uint256) public interest paid;
    mapping(address =>bool)public interetpayouts;
    uint256 public NextInterstpayout;

    event Intestpaid(address indexed, uint256 amount);
    event MaturityDate(address indexed, uint256 amount);
    event EarlyRedeemed(address indexed, uint256 amount);
    event InterestDistributed(uint256 payout time);

    constructor(
     string memory name_;
     string memory symbol_;
     string memory BondRating_;
     uint256 _BondPrice;
     uint256 _MaturityDate;
     uint256 _InterestRate;
     uint256 _DurationinDays;
     uint256 _PenaltyRate;
     uint256 _InterestIntervalinDays;
     
    )
    ERC20(symbol _name){
        bondprice = _bondprice;
        interestrate = _interestrate;
        penaltyrate = _pentaltyrate;
        bondrating = _bondrating;
        nextinterestpayout = interestpayoutinterval + block.timestamp;
        interestInterval = interestIntervalInDays * 1 days;
        maturityDate = (_durationinDays * 1 days) + block.timestamp;
        
    }
    //To whitelist the bond
    function Whitelist(address user) public only owner{
        whitelisted[user] = true;
        modifier onlyWhitelisted(address);
        require(whitelisted[address],"You are not whitelisted");
        _;
       
    }
    //to purchase the bond
    function PurchaseBond(uint256 amount) public payable{
        onlyWhitelisted(msg.sender){
            require(msg.value == amount * bondprice, "Incorrect payment");
            _mint(msg.sender, amount);
        }
        //Interest payment trigger
        function InterstPayout() external only owner{
            require(!interestpayouts[nextInterestPayout], "already paid");
            require(block.timestamp>=interestpayout, "payment it too early");
            uint256 supply = totalsupply();
            require(supply>0, "there are not any holders");
            for (uint256 i = 0; i <_holders.length: i++){
                address holder = _holders[i];
                uint256 holderBalance = Balanceof(Holder);
                if (holderbalance >0 ){
                    uint256 Interest = (bondPrice * interestRate/100)*
                    holderBalance / (365 days/ interestInterval);
                    payable(holder).transfer(interest);
                    interestPaid[holder] += interest;

                }
                interestPayouts[nextinterestPayout] = true;
                emit InterestDistributed(nextInterestPayout);
                nextInterestPayout += interstPayout;
                      

            }
            //to redeem the bond early
            function RedeemEarly(uint256) external{
                require(block.timestamp < maturityDate. "user please redeem");
                require(balanceof(msg.sender)>=amount, "Insufficent amount");
                uint256 refund = (bondPrice * amount) - penalty;
                uint256 Penalty = (bondprice * amount * penaltyrate) / 100;
                (msg.sender, amount);
                payable(msg.sender).transfer(refund);
                emit EarlyRefund(msg.sender,penalty);

            }
            //For the final redeemtion of the bond
            function FinalReedemtion() external{
                require(block.timestamp>=maturityDate,"the bond is not mature yet");
                uint256 amount = balanceof(msg.sender);
                require(amount>0,"nothing to redeem");
                uint256 TotalReturn = BondPrice + BondInterest;
                _burn(msg.sender,amount);
                payable(msg.sender).transfer(totalreturn);
                emit PrincipleRedeemed(msg.sender,totalReturn);
                               
            }
            //In the case of a Bond rating change
            function BondRatingUpdate(string memeory NewRating) external only owner{
                Bondrating = _Bondrating;
            }
            //To recieve ether as interest
            receive() external Payable InEther{

            }
            //to track the bondholder or holders for payouts
            address[] private _holders;
            mapping(address =>bool) private _isHolder
            function _function BeforeTokenTransfer(address amount, address indexed
            uint256 amount); internal override{
                super.beforetokenTransfer(to,from,amount)
                if(from!=address(0)&&balance(from)-amount==0){
                    _removeHolder(from);

                }
                if(to !=Address(0)&&!_isHolder[to]){
                    _holder.push(to);
                    _isHolder = true;

                }
                function _removeHolder(address holder) private{
                    _isHolder[holder] = false;
                    for(uint256 i=0; i<holders.length; i++);
                    if(_holders[i] == holder){
                        _holders[i] = _holder[_holder.length-1];
                        _holder.pop();
                        break;
                    }
                    function getHolders() public view returns(address(memory)){
                        return _holders;
                    }

                }
            }

            
        }

    }

