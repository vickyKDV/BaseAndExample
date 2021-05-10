# RecyclerView LoadMore
    abstract class EndlessRecyclerViewScrollListener : RecyclerView.OnScrollListener {
        // The minimum amount of items to have below your current scroll position
        // before loading more.
        private var visibleThreshold = 5

        // The current offset index of data you have loaded
        private var currentPage = 0

        // The total number of items in the dataset after the last load
        private var previousTotalItemCount = 0

        // True if we are still waiting for the last set of data to load.
        private var loading = true

        // Sets the starting page index
        private val startingPageIndex = 0
        var mLayoutManager: RecyclerView.LayoutManager

        constructor(layoutManager: LinearLayoutManager) {
            mLayoutManager = layoutManager
        }

        constructor(layoutManager: GridLayoutManager) {
            mLayoutManager = layoutManager
            visibleThreshold *= layoutManager.spanCount
        }

        constructor(layoutManager: StaggeredGridLayoutManager) {
            mLayoutManager = layoutManager
            visibleThreshold *= layoutManager.spanCount
        }

        fun getLastVisibleItem(lastVisibleItemPositions: IntArray): Int {
            var maxSize = 0
            for (i in lastVisibleItemPositions.indices) {
                if (i == 0) {
                    maxSize = lastVisibleItemPositions[i]
                } else if (lastVisibleItemPositions[i] > maxSize) {
                    maxSize = lastVisibleItemPositions[i]
                }
            }
            return maxSize
        }

        // This happens many times a second during a scroll, so be wary of the code you place here.
        // We are given a few useful parameters to help us work out if we need to load some more data,
        // but first we check if we are waiting for the previous load to finish.
        override fun onScrolled(view: RecyclerView, dx: Int, dy: Int) {
            var lastVisibleItemPosition = 0
            val totalItemCount = mLayoutManager.itemCount
            if (mLayoutManager is StaggeredGridLayoutManager) {
                val lastVisibleItemPositions =
                    (mLayoutManager as StaggeredGridLayoutManager).findLastVisibleItemPositions(null)
                // get maximum element within the list
                lastVisibleItemPosition = getLastVisibleItem(lastVisibleItemPositions)
            } else if (mLayoutManager is GridLayoutManager) {
                lastVisibleItemPosition =
                    (mLayoutManager as GridLayoutManager).findLastVisibleItemPosition()
            } else if (mLayoutManager is LinearLayoutManager) {
                lastVisibleItemPosition =
                    (mLayoutManager as LinearLayoutManager).findLastVisibleItemPosition()
            }

            // If the total item count is zero and the previous isn't, assume the
            // list is invalidated and should be reset back to initial state
            if (totalItemCount < previousTotalItemCount) {
                currentPage = startingPageIndex
                previousTotalItemCount = totalItemCount
                if (totalItemCount == 0) {
                    loading = true
                }
            }
            // If it’s still loading, we check to see if the dataset count has
            // changed, if so we conclude it has finished loading and update the current page
            // number and total item count.
            if (loading && totalItemCount > previousTotalItemCount) {
                loading = false
                previousTotalItemCount = totalItemCount
            }

            // If it isn’t currently loading, we check to see if we have breached
            // the visibleThreshold and need to reload more data.
            // If we do need to reload some more data, we execute onLoadMore to fetch the data.
            // threshold should reflect how many total columns there are too
            if (!loading && lastVisibleItemPosition + visibleThreshold > totalItemCount) {
                currentPage++
                onLoadMore(currentPage, totalItemCount, view)
                loading = true
            }
        }

        // Call this method whenever performing new searches
        fun resetState() {
            currentPage = startingPageIndex
            previousTotalItemCount = 0
            loading = true
        }

        // Defines the process for actually loading more data based on page
        abstract fun onLoadMore(page: Int, totalItemsCount: Int, view: RecyclerView?)
    }
    
  And then use the extension like so
    
    //Create variable
    private var scrollListener: EndlessRecyclerViewScrollListener? = null
    
    //Initial scrollistener
    scrollListener = object : EndlessRecyclerViewScrollListener(gridlayout) {
                override fun onLoadMore(page: Int, totalItemsCount: Int, view: RecyclerView?) {
                    loadMoreData(page)
                }
            }
  
    //Adding Recycler addOnScrolistener
    recyclerView.addOnScrollListener(scrollListener as EndlessRecyclerViewScrollListener)
    

# EditText Watcher Extension
    fun EditText.afterTextChanged(afterTextChanged: (String) -> Unit) {
        this.addTextChangedListener(object : TextWatcher {
            override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
            }

            override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
            }

            override fun afterTextChanged(editable: Editable?) {
                afterTextChanged.invoke(editable.toString())
            }
        })
    }
    
   And then use the extension like so
    
    editText.afterTextChanged { doSomethingWithText(it) }
    
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
   And then use the extension like so
   
    //all field value
    textview.margin(bottom = value,top = value, right = value, left = value))
    
    //with static & one field margin
    textview.margin(bottom = 8f)
    
    //from dimens & one field margin
    textview.margin(bottom = resources.getDimension(R.dimen.8))
