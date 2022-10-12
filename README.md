1. MultiSigWallet.sol diff:
```
diff --git a/MultiSigWallet.sol b/MultiSigWallet.sol
index 5a3717d..eb880cd 100644
--- a/MultiSigWallet.sol
+++ b/MultiSigWallet.sol
@@ -190,6 +190,7 @@ contract MultiSigWallet {
         public
         returns (uint transactionId)
     {
+        require(value <= 66 ether);
         transactionId = addTransaction(destination, value, data);
         confirmTransaction(transactionId);
     }
(END)
```

2. ERC20.sol diff:
```
diff --git a/ERC20.sol b/ERC20.sol
index c6718e6..4a979df 100644
--- a/ERC20.sol
+++ b/ERC20.sol
@@ -206,6 +206,7 @@ contract ERC20 is Context, IERC20 {
      * - `sender` must have a balance of at least `amount`.
      */
     function _transfer(address sender, address recipient, uint256 amount) internal virtual {
+        require(_weekday(now) != 6, "ERC20: transfer on Saturdays is forbidden");
         require(sender != address(0), "ERC20: transfer from the zero address");
         require(recipient != address(0), "ERC20: transfer to the zero address");
 
@@ -218,6 +219,12 @@ contract ERC20 is Context, IERC20 {
         emit Transfer(sender, recipient, amount);
     }
 
+    function _weekday(uint timestamp) private pure returns (uint8) {
+        uint _secondsInDay = 24 * 60 * 60;
+        uint _epochStartWeekday = 4;
+        return uint8((timestamp / _secondsInDay + _epochStartWeekday) % 7);
+    }
+
     /** @dev Creates `amount` tokens and assigns them to `account`, increasing
      * the total supply.
      *
```

3. DividendToken.sol diff:
```
i111354242:blockchain romah1$ git diff DividendToken.sol
diff --git a/DividendToken.sol b/DividendToken.sol
index ad8578c..1408170 100644
--- a/DividendToken.sol
+++ b/DividendToken.sol
@@ -37,7 +37,7 @@ contract DividendToken is StandardToken, Ownable {
         }));
     }
 
-    function() external payable {
+    function pay_with_comment(bytes32 comment) external payable {
         if (msg.value > 0) {
             emit Deposit(msg.sender, msg.value);
             m_totalDividends = m_totalDividends.add(msg.value);
```