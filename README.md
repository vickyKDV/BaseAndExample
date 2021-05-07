# Add Bullet Icon in Textview


    • = \u2022   
    ● = \u25CF  
    ○ = \u25CB   
    ▪ = \u25AA   
    ■ = \u25A0  
    □ = \u25A1   
    ► = \u25BA
    
    
 how to implement in Textview?
 
    setText("\u2022 Bullet")


# How set Margin Programatically in View With Dynamic function
    
### Create new Object Class and Copy Below Code and Paste to your Object Class 
    fun View.margin(
        left: Float? = null,
        top: Float? = null,
        right: Float? = null,
        bottom: Float? = null
    ) {
        layoutParams<ViewGroup.MarginLayoutParams> {
            left?.run { leftMargin = dpToPx(this) }
            top?.run { topMargin = dpToPx(this) }
            right?.run { rightMargin = dpToPx(this) }
            bottom?.run { bottomMargin = dpToPx(this) }
        }
    }

    inline fun <reified T : ViewGroup.LayoutParams> View.layoutParams(block: T.() -> Unit) {
        if (layoutParams is T) block(layoutParams as T)
    }

    fun View.dpToPx(dp: Float): Int = context.dpToPx(dp)
    fun Context.dpToPx(dp: Float): Int = TypedValue.applyDimension(
        TypedValue.COMPLEX_UNIT_DIP,
        dp,
        resources.displayMetrics
    ).toInt()
   how to implement ?
   
    //all field value
    textview.margin(bottom = value,top = value, right = value, left = value))
    
    //with static & one field margin
    textview.margin(bottom = 8f)
    
    //from dimens & one field margin
    textview.margin(bottom = resources.getDimension(R.dimen.8))
    
