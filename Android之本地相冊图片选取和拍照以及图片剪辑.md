PhotoUtils.java



```java
public class PhotoUtil {
	public static final int NONE = 0;
	public static final String IMAGE_UNSPECIFIED = "image/*";//随意图片类型
	public static final int PHOTOGRAPH = 1;// 拍照
	public static final int PHOTOZOOM = 2; // 缩放
	public static final int PHOTORESOULT = 3;// 结果
	public static final int PICTURE_HEIGHT = 500;
	public static final int PICTURE_WIDTH = 500;
	public static String imageName;
	
	/**
	 * 从系统相冊中选取照片上传
	 * @param activity
	 */
	public static void selectPictureFromAlbum(Activity activity){
		// 调用系统的相冊
		Intent intent = new Intent(Intent.ACTION_PICK, null);
		intent.setDataAndType(
				MediaStore.Images.Media.EXTERNAL_CONTENT_URI,
				IMAGE_UNSPECIFIED);

		// 调用剪切功能
		activity.startActivityForResult(intent, PHOTOZOOM);
	}
	
	/**
	 * 从系统相冊中选取照片上传
	 * @param fragment
	 */
	public static void selectPictureFromAlbum(Fragment fragment){
		// 调用系统的相冊
		Intent intent = new Intent(Intent.ACTION_PICK, null);
		intent.setDataAndType(
				MediaStore.Images.Media.EXTERNAL_CONTENT_URI,
				IMAGE_UNSPECIFIED);
		
		// 调用剪切功能
		fragment.startActivityForResult(intent, PHOTOZOOM);
	}
	
	/**
	 * 拍照
	 * @param activity
	 */
	public static void photograph(Activity activity){
		imageName = File.separator + getStringToday() + ".jpg";

		// 调用系统的拍照功能
		Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
		String status = Environment.getExternalStorageState();
		if(status.equals(Environment.MEDIA_MOUNTED)){
			intent.putExtra(MediaStore.EXTRA_OUTPUT, Uri.fromFile(new File(
					Environment.getExternalStorageDirectory(), imageName)));
		}else{
			intent.putExtra(MediaStore.EXTRA_OUTPUT, Uri.fromFile(new File(
					activity.getFilesDir(), imageName)));
		}
		activity.startActivityForResult(intent, PHOTOGRAPH);
	}
	
	/**
	 * 拍照
	 * @param fragment
	 */
	public static void photograph(Fragment fragment){
		imageName = "/" + getStringToday() + ".jpg";
		
		// 调用系统的拍照功能
		Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
		String status = Environment.getExternalStorageState();
		if(status.equals(Environment.MEDIA_MOUNTED)){
			intent.putExtra(MediaStore.EXTRA_OUTPUT, Uri.fromFile(new File(
					Environment.getExternalStorageDirectory(), imageName)));
		}else{
			intent.putExtra(MediaStore.EXTRA_OUTPUT, Uri.fromFile(new File(
					fragment.getActivity().getFilesDir(), imageName)));
		}
		fragment.startActivityForResult(intent, PHOTOGRAPH);
	}
	
	/**
	 * 图片裁剪
	 * @param activity
	 * @param uri
	 * @param height
	 * @param width
	 */
	public static void startPhotoZoom(Activity activity,Uri uri,int height,int width) {
		Intent intent = new Intent("com.android.camera.action.CROP");
		intent.setDataAndType(uri, IMAGE_UNSPECIFIED);
		intent.putExtra("crop", "true");
		// aspectX aspectY 是宽高的比例
		intent.putExtra("aspectX", 1);
		intent.putExtra("aspectY", 1);
		// outputX outputY 是裁剪图片宽高
		intent.putExtra("outputX", height);
		intent.putExtra("outputY", width);
		intent.putExtra("noFaceDetection", true); //关闭人脸检測
		intent.putExtra("return-data", true);//假设设为true则返回bitmap
		intent.putExtra(MediaStore.EXTRA_OUTPUT, uri);//输出文件
		intent.putExtra("outputFormat", Bitmap.CompressFormat.JPEG.toString());
		activity.startActivityForResult(intent, PHOTORESOULT);
	}
	
	/**
	 * 图片裁剪
	 * @param activity	
	 * @param uri	  	原图的地址
	 * @param height  	指定的剪辑图片的高
	 * @param width	  	指定的剪辑图片的宽
	 * @param destUri 	剪辑后的图片存放地址
	 */
	public static void startPhotoZoom(Activity activity,Uri uri,int height,int width,Uri destUri) {
		Intent intent = new Intent("com.android.camera.action.CROP");
		intent.setDataAndType(uri, IMAGE_UNSPECIFIED);
		intent.putExtra("crop", "true");
		// aspectX aspectY 是宽高的比例
		intent.putExtra("aspectX", 1);
		intent.putExtra("aspectY", 1);
		// outputX outputY 是裁剪图片宽高
		intent.putExtra("outputX", height);
		intent.putExtra("outputY", width);
		intent.putExtra("noFaceDetection", true); //关闭人脸检測
		intent.putExtra("return-data", false);//假设设为true则返回bitmap
		intent.putExtra(MediaStore.EXTRA_OUTPUT, destUri);//输出文件
		intent.putExtra("outputFormat", Bitmap.CompressFormat.JPEG.toString());
		activity.startActivityForResult(intent, PHOTORESOULT);
	}
	
	/**
	 * 图片裁剪
	 * @param fragment
	 * @param uri
	 * @param height
	 * @param width
	 */
	public static void startPhotoZoom(Fragment fragment,Uri uri,int height,int width) {
		Intent intent = new Intent("com.android.camera.action.CROP");
		intent.setDataAndType(uri, IMAGE_UNSPECIFIED);
		intent.putExtra("crop", "true");
		// aspectX aspectY 是宽高的比例
		intent.putExtra("aspectX", 1);
		intent.putExtra("aspectY", 1);
		// outputX outputY 是裁剪图片宽高
		intent.putExtra("outputX", height);
		intent.putExtra("outputY", width);
		intent.putExtra("return-data", true);
		fragment.startActivityForResult(intent, PHOTORESOULT);
	}
	
	/**
	 * 获取当前系统时间并格式化
	 * @return
	 */
	@SuppressLint("SimpleDateFormat")
	public static String getStringToday() {
		Date currentTime = new Date();
		SimpleDateFormat formatter = new SimpleDateFormat("yyyyMMddHHmmss");
		String dateString = formatter.format(currentTime);
		return dateString;
	}
	
	/**
 	 * 制作图片的路径地址
 	 * @param context
 	 * @return
 	 */
 	public static String getPath(Context context){
 		String path = null;
 		File file = null;
 		long tag = System.currentTimeMillis();
 		if(Environment.MEDIA_MOUNTED.equals(Environment.getExternalStorageState())){
 			//SDCard是否可用
 			path = Environment.getExternalStorageDirectory() + File.separator +"myimages/";
 			file = new File(path);
 			if(!file.exists()){
 				file.mkdirs();
 			}
 			path = Environment.getExternalStorageDirectory() + File.separator +"myimages/"+ tag + ".png";
 		}else{
 			path = context.getFilesDir() + File.separator +"myimages/";
 			file = new File(path);
 			if(!file.exists()){
 				file.mkdirs();
 			}
 			path = context.getFilesDir() + File.separator +"myimages/"+ tag + ".png";
 		}
		return path;
 	}
 	
 	/**
 	 * 按比例获取bitmap
 	 * @param path
 	 * @param w
 	 * @param h
 	 * @return
 	 */
 	public static Bitmap convertToBitmap(String path, int w, int h) {
        BitmapFactory.Options opts = new BitmapFactory.Options();
        // 设置为ture仅仅获取图片大小
        opts.inJustDecodeBounds = true;
        opts.inPreferredConfig = Bitmap.Config.ARGB_8888;
        BitmapFactory.decodeFile(path, opts);
        int width = opts.outWidth;
        int height = opts.outHeight;
        float scaleWidth = 0.f, scaleHeight = 0.f;
        if (width > w || height > h) {
            // 缩放
            scaleWidth = ((float) width) / w;
            scaleHeight = ((float) height) / h;
        }
        opts.inJustDecodeBounds = false;
        float scale = Math.max(scaleWidth, scaleHeight);
        opts.inSampleSize = (int)scale;
        WeakReference<Bitmap> weak = new WeakReference<Bitmap>(BitmapFactory.decodeFile(path, opts));
        return Bitmap.createScaledBitmap(weak.get(), w, h, true);
    }
 	
 	/**
 	 * 获取原图bitmap
 	 * @param path
 	 * @return
 	 */
 	public static Bitmap convertToBitmap2(String path) {
        BitmapFactory.Options opts = new BitmapFactory.Options();
        // 设置为ture仅仅获取图片大小
        opts.inJustDecodeBounds = true;
        opts.inPreferredConfig = Bitmap.Config.ARGB_8888;
        // 返回为空
        BitmapFactory.decodeFile(path, opts);
        return  BitmapFactory.decodeFile(path, opts);
    }
}
```



ImageUtils.java



```java
public class ImageUtils {

	private static final String TAG = ImageUtils.class.getSimpleName();
	
	
	/**
	 * 依据Uri获取路径
	 * @param contentUri
	 * @return
	 */
	public static String getRealPathByURI(Uri contentUri,Context context) {
		String res = null;
		String[] proj = { MediaStore.Images.Media.DATA };
		Cursor cursor = context.getContentResolver().query(contentUri,
				proj, null, null, null);
		if (cursor.moveToFirst()) {
			;
			int column_index = cursor
					.getColumnIndexOrThrow(MediaStore.Images.Media.DATA);
			res = cursor.getString(column_index);
		}
		cursor.close();
		return res;
	}

	/**
	 * 创建一条图片地址uri,用于保存拍照后的照片
	 * 
	 * @param context
	 * @return 图片的uri
	 */
	public static Uri createImagePathUri(Context context) {
		Uri imageFilePath = null;
		String status = Environment.getExternalStorageState();
		SimpleDateFormat timeFormatter = new SimpleDateFormat(
				"yyyyMMdd_HHmmss", Locale.CHINA);
		long time = System.currentTimeMillis();
		String imageName = timeFormatter.format(new Date(time));
		// ContentValues是我们希望这条记录被创建时包括的数据信息
		ContentValues values = new ContentValues(3);
		values.put(MediaStore.Images.Media.DISPLAY_NAME, imageName);
		values.put(MediaStore.Images.Media.DATE_TAKEN, time);
		values.put(MediaStore.Images.Media.MIME_TYPE, "image/jpg");
		if (status.equals(Environment.MEDIA_MOUNTED)) {// 推断是否有SD卡,优先使用SD卡存储,当没有SD卡时使用手机存储
			imageFilePath = context.getContentResolver().insert(
					MediaStore.Images.Media.EXTERNAL_CONTENT_URI, values);
		} else {
			imageFilePath = context.getContentResolver().insert(
					MediaStore.Images.Media.INTERNAL_CONTENT_URI, values);
		}
		Log.i("", "生成的照片输出路径：" + imageFilePath.toString());
		return imageFilePath;
	}

	/**
	 * 图片压缩
	 * 
	 * @param bmp
	 * @param file
	 */
	public static void compressBmpToFile(File file,int height,int width) {
		Bitmap bmp = decodeSampledBitmapFromFile(file.getPath(), height, width);
		ByteArrayOutputStream baos = new ByteArrayOutputStream();
		int options = 100;
		bmp.compress(Bitmap.CompressFormat.JPEG, options, baos);
		/*while (baos.toByteArray().length / 1024 > 30) {
			baos.reset();
			if (options - 10 > 0) {
				options = options - 10;
				bmp.compress(Bitmap.CompressFormat.JPEG, options, baos);
			}
			if (options - 10 <= 0) {
				break;
			}
		}*/
		try {
			FileOutputStream fos = new FileOutputStream(file);
			fos.write(baos.toByteArray());
			fos.flush();
			fos.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	/**
	 * 将图片变成bitmap
	 * 
	 * @param path
	 * @return
	 */
	public static Bitmap getImageBitmap(String path) {
		Bitmap bitmap = null;
		File file = new File(path);
		if (file.exists()) {
			bitmap = BitmapFactory.decodeFile(path);
			return bitmap;
		}
		return null;
	}

    //=================================图片压缩方法===============================================
	
	/**
     * 质量压缩
     * @author ping 2015-1-5 下午1:29:58
     * @param image
     * @param maxkb
     * @return
     */
    public static Bitmap compressBitmap(Bitmap image,int maxkb) {
        //L.showlog(压缩图片);
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        // 质量压缩方法，这里100表示不压缩。把压缩后的数据存放到baos中
        image.compress(Bitmap.CompressFormat.JPEG, 50, baos);
        int options = 100;
        // 循环推断假设压缩后图片是否大于(maxkb)50kb,大于继续压缩
        while (baos.toByteArray().length / 1024 > maxkb) { 
        	// 重置baos即清空baos
            baos.reset();
            if(options-10>0){
            	// 每次都降低10
            	options -= 10;
            }
            // 这里压缩options%。把压缩后的数据存放到baos中
            image.compress(Bitmap.CompressFormat.JPEG, options, baos);
        }
        // 把压缩后的数据baos存放到ByteArrayInputStream中
        ByteArrayInputStream isBm = new ByteArrayInputStream(baos.toByteArray());
        // 把ByteArrayInputStream数据生成图片
        Bitmap bitmap = BitmapFactory.decodeStream(isBm, null, null);
        return bitmap;
    }
     
    /**
     * 
     * @param res
     * @param resId
     * @param reqWidth
     *            所需图片压缩尺寸最小宽度
     * @param reqHeight
     *            所需图片压缩尺寸最小高度
     * @return
     */
    public static Bitmap decodeSampledBitmapFromResource(Resources res,
            int resId, int reqWidth, int reqHeight) {
        final BitmapFactory.Options options = new BitmapFactory.Options();
        options.inJustDecodeBounds = true;
        BitmapFactory.decodeResource(res, resId, options);
         
        options.inSampleSize = calculateInSampleSize(options, reqWidth,
                reqHeight);
        options.inJustDecodeBounds = false;
        return BitmapFactory.decodeResource(res, resId, options);
    }
 
    /**
     * 
     * @param filepath
     * 			 图片路径
     * @param reqWidth
     *			所需图片压缩尺寸最小宽度
     * @param reqHeight
     *          所需图片压缩尺寸最小高度
     * @return
     */
    public static Bitmap decodeSampledBitmapFromFile(String filepath,int reqWidth, int reqHeight) {
        final BitmapFactory.Options options = new BitmapFactory.Options();
        options.inJustDecodeBounds = true;
        BitmapFactory.decodeFile(filepath, options);
 
        options.inSampleSize = calculateInSampleSize(options, reqWidth,
                reqHeight);
        options.inJustDecodeBounds = false;
        return BitmapFactory.decodeFile(filepath, options);
    }
 
    /**
     * 
     * @param bitmap
     * @param reqWidth
     * 			所需图片压缩尺寸最小宽度
     * @param reqHeight
     * 			所需图片压缩尺寸最小高度
     * @return
     */
    public static Bitmap decodeSampledBitmapFromBitmap(Bitmap bitmap,
            int reqWidth, int reqHeight) {
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        bitmap.compress(Bitmap.CompressFormat.PNG, 90, baos);
        byte[] data = baos.toByteArray();
         
        final BitmapFactory.Options options = new BitmapFactory.Options();
        options.inJustDecodeBounds = true;
        BitmapFactory.decodeByteArray(data, 0, data.length, options);
        options.inSampleSize = calculateInSampleSize(options, reqWidth,
                reqHeight);
        options.inJustDecodeBounds = false;
        return BitmapFactory.decodeByteArray(data, 0, data.length, options);
    }
 
    /**
     * 计算压缩比例值(改进版 by touch_ping)
     * 
     * 原版2>4>8...倍压缩
     * 当前2>3>4...倍压缩
     * 
     * @param options
     *            解析图片的配置信息
     * @param reqWidth
     *            所需图片压缩尺寸最小宽度O
     * @param reqHeight
     *            所需图片压缩尺寸最小高度
     * @return
     */
    public static int calculateInSampleSize(BitmapFactory.Options options,
            int reqWidth, int reqHeight) {
         
        final int picheight = options.outHeight;
        final int picwidth = options.outWidth;
         
        int targetheight = picheight;
        int targetwidth = picwidth;
        int inSampleSize = 1;
         
        if (targetheight > reqHeight || targetwidth > reqWidth) {
            while (targetheight  >= reqHeight
                    && targetwidth>= reqWidth) {
                inSampleSize += 1;
                targetheight = picheight/inSampleSize;
                targetwidth = picwidth/inSampleSize;
            }
        }
         
        Log.i("===","终于压缩比例:" +inSampleSize + "倍");
        Log.i("===", "新尺寸:" +  targetwidth + "*" +targetheight);
        return inSampleSize;
    }
    
    // 读取图像的旋转度
 	public static int readBitmapDegree(String path) {
 		int degree = 0;
 		try {
 			ExifInterface exifInterface = new ExifInterface(path);
 			int orientation = exifInterface.getAttributeInt(
 					ExifInterface.TAG_ORIENTATION,
 					ExifInterface.ORIENTATION_NORMAL);
 			switch (orientation) {
 			case ExifInterface.ORIENTATION_ROTATE_90:
 				degree = 90;
 				break;
 			case ExifInterface.ORIENTATION_ROTATE_180:
 				degree = 180;
 				break;
 			case ExifInterface.ORIENTATION_ROTATE_270:
 				degree = 270;
 				break;
 			}
 		} catch (IOException e) {
 			e.printStackTrace();
 		}
 		return degree;
 	}

 	
 	/**
 	 * 将图片依照某个角度进行旋转
 	 *
 	 * @param bm
 	 *            须要旋转的图片
 	 * @param degree
 	 *            旋转角度
 	 * @return 旋转后的图片
 	 */
 	public static Bitmap rotateBitmapByDegree(Bitmap bm, int degree) {
 	    Bitmap returnBm = null;
 	  
 	    // 依据旋转角度，生成旋转矩阵
 	    Matrix matrix = new Matrix();
 	    matrix.postRotate(degree);
 	    try {
 	        // 将原始图片依照旋转矩阵进行旋转，并得到新的图片
 	        returnBm = Bitmap.createBitmap(bm, 0, 0, bm.getWidth(), bm.getHeight(), matrix, true);
 	    } catch (OutOfMemoryError e) {
 	    }
 	    if (returnBm == null) {
 	        returnBm = bm;
 	    }
 	    if (bm != returnBm) {
 	        bm.recycle();
 	    }
 	    return returnBm;
 	}
 	
 	/**
 	 * 
 	 * @param mBitmap
 	 * @param fileName
 	 */
 	public static void saveBitmapToLocal(Bitmap mBitmap,String fileName) {
		if(mBitmap != null){
				FileOutputStream fos = null;
				try {
					File file = new File(fileName);
					if(file.exists()){
						file.delete();
					}
					file.createNewFile();
					fos = new FileOutputStream(file);
					mBitmap.compress(Bitmap.CompressFormat.PNG, 80, fos);
					fos.flush();
				} catch (Exception e) {
					e.printStackTrace();
				}finally{
					
					try {
						if(fos != null){
							fos.close();
						}
					} catch (IOException e) {
						e.printStackTrace();
					}
				}
				
		}
	}
 	
 	/**
 	 * 将下载下来的图片保存到SD卡或者本地.并返回图片的路径(包括文件命和扩展名)
 	 * @param context
 	 * @param bitName
 	 * @param mBitmap
 	 * @return
 	 */
	public static String saveBitmap(Context context,String bitName, Bitmap mBitmap) {
		String path = null;
		File f;
		if(mBitmap != null){
			if(Environment.MEDIA_MOUNTED.equals(Environment.getExternalStorageState())){
				f = new File(Environment.getExternalStorageDirectory() + File.separator +"images/");
				String fileName = Environment.getExternalStorageDirectory() + File.separator +"images/"+ bitName + ".png";
				path = fileName;
				FileOutputStream fos = null;
				try {
					if(!f.exists()){
						f.mkdirs();
					}
					File file = new File(fileName);
					file.createNewFile();
					fos = new FileOutputStream(file);
					mBitmap.compress(Bitmap.CompressFormat.PNG, 100, fos);
					fos.flush();
				} catch (Exception e) {
					e.printStackTrace();
				}finally{
					
					try {
						if(fos != null){
							fos.close();
						}
					} catch (IOException e) {
						e.printStackTrace();
					}
				}
				
			}else{
				//本地存储路径
				f = new File(context.getFilesDir() + File.separator +"images/");
				Log.i(TAG, "本地存储路径:"+context.getFilesDir() + File.separator +"images/"+ bitName + ".png");
				path = context.getFilesDir() + File.separator +"images/"+ bitName + ".png";
				FileOutputStream fos = null;
				try {
					if(!f.exists()){
						f.mkdirs();
					}
					File file = new File(path);
					file.createNewFile();
					fos = new FileOutputStream(file);
					mBitmap.compress(Bitmap.CompressFormat.PNG, 100, fos);
					fos.flush();
				} catch (IOException e) {
					e.printStackTrace();
				}finally{
					try {
						if(fos != null){
							fos.close();
						}
					} catch (IOException e) {
						e.printStackTrace();
					}
				}
				
			}
		}
		
		return path;
	}
	
	/**
	 * 删除图片
	 * @param context
	 * @param bitName
	 */
	public void deleteFile(Context context,String bitName) {  
		if(Environment.MEDIA_MOUNTED.equals(Environment.getExternalStorageState())){
			File dirFile = new File(Environment.getExternalStorageDirectory() + File.separator + "images/"+ bitName + ".png"); 
			if (!dirFile.exists()) {
				return;
			}
			
			dirFile.delete();
		} else {
			File f = new File(context.getFilesDir() + File.separator
					+ "images/" + bitName + ".png");
			if(!f.exists()){
				return;
			}
			f.delete();
		}
	}	
}
```



demo



```
public class MainActivity extends Activity implements OnClickListener {

	private static final String TAG = MainActivity.class.getSimpleName();

	private ImageView smallImg;
	private ImageView clipImg;
	private TextView tv1,tv2;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		findViewById(R.id.options).setOnClickListener(this);
		initView();
	}

	protected void initView() {
		smallImg = (ImageView) findViewById(R.id.small_img);
		clipImg = (ImageView) findViewById(R.id.clip_pic);
		tv1 = (TextView) findViewById(R.id.small_img_title);
		tv2 = (TextView) findViewById(R.id.clip_pic_title);
	}

	@Override
	public void onClick(View v) {
		if (v.getId() == R.id.options) {
			options();
		}
	}

	private String path;

	@Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		if (resultCode == PhotoUtil.NONE)
			return;
		// 拍照
		if (requestCode == PhotoUtil.PHOTOGRAPH) {
			// 设置文件保存路径这里放在跟文件夹下
			File picture = null;
			if (Environment.MEDIA_MOUNTED.equals(Environment.getExternalStorageState())) {
				picture = new File(Environment.getExternalStorageDirectory() + PhotoUtil.imageName);
				if (!picture.exists()) {
					picture = new File(Environment.getExternalStorageDirectory() + PhotoUtil.imageName);
				}
			} else {
				picture = new File(this.getFilesDir() + PhotoUtil.imageName);
				if (!picture.exists()) {
					picture = new File(MainActivity.this.getFilesDir() + PhotoUtil.imageName);
				}
			}

			path = PhotoUtil.getPath(this);// 生成一个地址用于存放剪辑后的图片
			if (TextUtils.isEmpty(path)) {
				Log.e(TAG, "随机生成的用于存放剪辑后的图片的地址失败");
				return;
			}
			Uri imageUri = UriPathUtils.getUri(this, path);
			PhotoUtil.startPhotoZoom(MainActivity.this, Uri.fromFile(picture), PhotoUtil.PICTURE_HEIGHT, PhotoUtil.PICTURE_WIDTH, imageUri);
		}

		if (data == null)
			return;

		// 读取相冊缩放图片
		if (requestCode == PhotoUtil.PHOTOZOOM) {

			path = PhotoUtil.getPath(this);// 生成一个地址用于存放剪辑后的图片
			if (TextUtils.isEmpty(path)) {
				Log.e(TAG, "随机生成的用于存放剪辑后的图片的地址失败");
				return;
			}
			Uri imageUri = UriPathUtils.getUri(this, path);
			PhotoUtil.startPhotoZoom(MainActivity.this, data.getData(), PhotoUtil.PICTURE_HEIGHT, PhotoUtil.PICTURE_WIDTH, imageUri);
		}
		// 处理结果
		if (requestCode == PhotoUtil.PHOTORESOULT) {
			/**
			 * 在这里处理剪辑结果。能够获取缩略图，获取剪辑图片的地址。得到这些信息能够选则用于上传图片等等操作
			 * */

			/**
			 * 如。依据path获取剪辑后的图片
			 */
			Bitmap bitmap = PhotoUtil.convertToBitmap(path,PhotoUtil.PICTURE_HEIGHT, PhotoUtil.PICTURE_WIDTH);
			if(bitmap != null){
				tv2.setText(bitmap.getHeight()+"x"+bitmap.getWidth()+"图");
				clipImg.setImageBitmap(bitmap);
			}
			
			Bitmap bitmap2 = PhotoUtil.convertToBitmap(path,120, 120);
			if(bitmap2 != null){
				tv1.setText(bitmap2.getHeight()+"x"+bitmap2.getWidth()+"图");
				smallImg.setImageBitmap(bitmap2);
			}
			
//			Bundle extras = data.getExtras();
//			if (extras != null) {
//				Bitmap photo = extras.getParcelable("data");
//				ByteArrayOutputStream stream = new ByteArrayOutputStream();
//				photo.compress(Bitmap.CompressFormat.JPEG, 100, stream);// (0-100)压缩文件
//				InputStream isBm = new ByteArrayInputStream(stream.toByteArray());
//			}

		}
		super.onActivityResult(requestCode, resultCode, data);
	}

	protected void options() {
		ActionSheetDialog mDialog = new ActionSheetDialog(this).builder();
		mDialog.setTitle("选择");
		mDialog.setCancelable(false);
		mDialog.addSheetItem("拍照", SheetItemColor.Blue, new OnSheetItemClickListener() {
			@Override
			public void onClick(int which) {
				PhotoUtil.photograph(MainActivity.this);
			}
		}).addSheetItem("从相冊选取", SheetItemColor.Blue, new OnSheetItemClickListener() {
			@Override
			public void onClick(int which) {
				PhotoUtil.selectPictureFromAlbum(MainActivity.this);
			}
		}).show();
	}

	@Override
	protected void onDestroy() {
		super.onDestroy();
	}
}
```

