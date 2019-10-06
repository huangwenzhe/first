public class Sort {

    public static void insertSort(int []array){
        for(int i = 0 ; i<array.length;i++){
            int key = array[i];
            int j ;
            for(j=i-1;j>=0&&array[j]>key;j--){
                    array[j+1] = array[j];
            }
            array[j+1] = key;
        }
    }
    public static void shellSort(int []array){
        int gap = array.length;
        while(gap>1){
            InsertSortWithGap(array,gap);
            gap = gap/2;
        }
        InsertSortWithGap(array,1);
    }

    private static void InsertSortWithGap(int[] array, int gap) {
        for(int i = gap ;i<array.length;i++){
            int k = array[i];
            int j ;
            for(j = i-gap;j>=0;j-=gap){
                if(array[j]>k){
                    array[j+gap]= array[j];
                }else{
                    break;
                }
            }
            array[j+gap]=k;
        }
    }
    public static void selectSort(int []array){
        for(int i = 0;i<array.length-1;i++){
            int min = i;
            for(int j = 1+i;j<array.length;j++){
                if(array[min]>array[j]){
                    min = j;
                }
            }
            swap(array,min,i);
        }
    }
    public static void selectSortBig(int []array){
        for(int i =0;i<array.length-1;i++){
            int max = 0;
            for(int j = 1;j<array.length-i;j++){
                if(array[j]>array[max]){
                    max = j;
                }
            }
            swap(array,max,array.length-1-i);
        }
    }
    private static void swap(int[] array, int max, int i) {
        int t = array[max];
        array[max] = array[i];
        array[i] = t;
    }

    public static void selectSortOp(int []array){
        int low = 0;
        int high  = array.length-1;
        while(low<=high){
            int min = low;
            int max = low;
            for(int i = low+1;i<=high;i++){
                if(array[i]<array[min]){
                    min = i;
                }
                if(array[i]>array[max]){
                    max = i;
                }
            }
            swap(array,min,low);
            if(max == low)
                max = min;
            swap(array,max,high);
            low++;
            high--;
        }
    }
    public static void heapSort(int []array){
        CreatHeap(array);
        for(int i = 0;i<array.length-1;i++){
            swap(array,0,array.length-1-i);//建立大堆  将最后个数和大堆的堆首交换  再进行向下调整
            shifDownBig(array,array.length-1-i,0);
        }
    }
    public static void CreatHeap(int []array){
        for(int i =(array.length-2)/2;i>=0;i--){  //最后1个的结点的双亲结点
            shifDownBig(array,array.length,i);
        }
    }
    public static void shifDownBig(int []array,int size,int index){
        int left = 2*index+1;
        while(left<size) {
            int right = left + 1;
            int max = left;
            if (right < size) {
                if (array[left] < array[right]) max = right;
            }
            if(array[max]<=array[index])
                break;

            swap(array,index,max);
            index = max;
            left = 2*index+1;
        }
    }
    public static void bubbleSort(int []array){
        for(int i = 0;i<array.length;i++){
            for(int j = 0;j<i;j++){
                if(array[i]<array[j]){
                    swap(array,i,j);
                }
            }
        }
    }

    public static  void qulicSort(int []array){
        quickSortINter(array,0,array.length-1);
    }
    public static void quickSortINter(int []array,int left ,int right){
        if(right<=left)
            return ;
        int val  = patition3x(array,left,right);
        quickSortINter(array,left,val-1);
        quickSortINter(array,val+1,right);
    }
    public static int patition3x(int []array, int left ,int right){
        int reference_Val = array[right];
        int d = right-1;
        for(int i =right-1;i>=0;i--){
            if(array[i]>reference_Val){
                swap(array,d,i);
                d--;
            }
        }
        swap(array,d+1,right);
        return d+1;
    }
    private static int patition(int[] array, int left, int right) {
        int began = left;
        int end = right;
        int maink = array[left];
        while(end > began){
            while(end > began && array[end] >= maink){
                end --;
            }
            while (end > began && maink >= array[began]){
                began ++;
            }

            swap(array,end,began);
        }
        swap(array,left,began);
        return end;
    }
    public static int patition2(int []array,int left ,int right){
        int begin = left;
        int end = right;
        int reference_val = array[left];
        while(end>begin){
            while(end>begin && array[end]>=reference_val){
                end --;
            }
            array[begin] = array[end];
            while(end>begin && array[begin]<= reference_val){
                begin ++;
            }
            array[end] = array[begin];
        }
        array[begin] = reference_val;
        return  begin;
    }
    public static int patition3 (int[]array, int left, int right){
        int reference_val = array[left];
        int d = left+1;
        for(int i = left+1;i<=right;++i){
            if(array[i]<reference_val){
                swap(array,i,d++);
            }
        }
        swap(array,d-1,left);
        return d-1;
    }
    public static  void mergeSort(int []array){   //归并
        int []extra = new int [array.length];
        mergeINter(array,0,array.length,extra);
    }

    private static void mergeINter(int[] array, int low, int high, int[] extra) {
        if(low >=high-1){
            return;
        }
        int mid = (low+high)/2;
        mergeINter(array,low,mid,extra);
        mergeINter(array,mid,high,extra);
        merge(array,low ,mid ,high,extra);
    }

    public static void merge(int []array,int low ,int mid, int high,int []extra){
        int i = low;
        int j = mid;
        int len = high- low;
        int k =0;
        while(i<mid&&j<high){
            if(array[i]>array[j]){
                extra[k++] = array[j++];
            }
            else {
                extra[k++] = array[i++];
            }
        }
        while(i<mid){
            extra[k++] = array[i++];
        }while(j<high){
            extra[k++] = array[j++];
        }
        for(int m = 0; m <len;m++){
            array[low+m] = extra[m];
         }
    }
