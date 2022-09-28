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
+        require(value <= 66);
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

3. 