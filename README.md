## Sample of the working python script.
#### Code from [PR](https://github.com/JetBrains/intellij-community/pull/2569): 
 ``` 
 @@ -37,16 +37,22 @@ open class AddModifierFix(
     protected fun invokeOnElement(element: KtModifierListOwner?) {
         element?.addModifier(modifier)
 
-        if (modifier == KtTokens.ABSTRACT_KEYWORD && (element is KtProperty || element is KtNamedFunction)) {
-            element.containingClass()?.run {
-                if (!hasModifier(KtTokens.ABSTRACT_KEYWORD) && !hasModifier(KtTokens.SEALED_KEYWORD)) {
-                    addModifier(KtTokens.ABSTRACT_KEYWORD)
+        when (modifier) {
+            KtTokens.ABSTRACT_KEYWORD -> {
+                if (element is KtProperty || element is KtNamedFunction) {
+                    element.containingClass()?.run {
+                        if (!hasModifier(KtTokens.ABSTRACT_KEYWORD) && !hasModifier(KtTokens.SEALED_KEYWORD)) {
+                            addModifier(KtTokens.ABSTRACT_KEYWORD)
+                        }
+                    }
                 }
             }
-        }
 
-        if (modifier == KtTokens.NOINLINE_KEYWORD) {
-            element?.removeModifier(KtTokens.CROSSINLINE_KEYWORD)
+            KtTokens.OVERRIDE_KEYWORD ->
+                element?.removeModifier(KtTokens.PRIVATE_KEYWORD) 
 ```
#### Original Comment: 
 It is not specifically about private. Basically, quickfix should never lead to CANNOT_WEAKEN_ACCESS_PRIVILEGE error, which may occur with any modifier except public.

#### CodeReviewer Comment:
This is a good change, but I think it's a good idea to add a `TODO` here.

---

