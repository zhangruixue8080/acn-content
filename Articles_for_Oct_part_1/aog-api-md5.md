# 如何调用 API 获取 File 文件的 MD5 #

**问题：** 如何调用 API 获取 File 文件的 MD5

**现象：** 文件以 Blob 形式存储，可以通过 API 取其 MD5 值。存储在 File 中，则提供的 API 获取 MD5 值为空

1. .NET SDK 测试：

	- 测试代码：

			//file 文件测试
			String filePath1 = "E:\\Background\\linux-087.jpg";
			CloudFile cloudFile1 = rootDir.GetFileReference("image-03.jpg");
			cloudFile1.UploadFromFile(filePath1,FileMode.OpenOrCreate);   
			Console.WriteLine("md5="+cloudFile1.Properties.ContentMD5);
			
			//blob 文件测试
			CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
			CloudBlobContainer container = blobClient.GetContainerReference("file");
			CloudBlockBlob blob = container.GetBlockBlobReference(@"image-03.jpg");
			blob.UploadFromFile(filePath1, FileMode.OpenOrCreate);
			Console.WriteLine("md5=" + blob.Properties.ContentMD5);

	- 测试结果：

	![.net-test-result](media/aog-api-md5/net-test-result.png ".net-test-result")

2. JAVA SDK 测试：

	- 测试代码：	

    		//file 文件测试
    		final String filePath = "E:\\Background\\linux-087.jpg";
    		CloudFile cloudFile = rootDir.getFileReference("image-04.jpg");
    		cloudFile.uploadFromFile(filePath); 
    		System.out.println("md5="+cloudFile.getProperties().getContentMD5());
    		
    		//blob 文件测试
    		CloudBlobClient blobClient = account.createCloudBlobClient();
    		CloudBlobContainer container = blobClient.getContainerReference("file");
    		CloudBlockBlob blobFile = container.getBlockBlobReference("image-04.jpg");
    		blobFile.uploadFromFile(filePath);
    		System.out.println("md5="+blobFile.getProperties().getContentMD5());
 
	- 测试结果：

	![java-test-result](media/aog-api-md5/java-test-result.png "java-test-result")


**问题原因：** 由于 File 本身应用形式为磁盘，而文件的一些更新等动作会导致 MD5 重新计算，这对于磁盘能影响较大，因此暂不支持 MD5 自动生成。而 Block Blob 本身而言并不支持修改操作，文件的 MD5 值相对稳定。参考：[https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-files/#develop-with-file-storage](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-files/#develop-with-file-storage  "https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-files/#develop-with-file-storage ") 

**解决方法：** 如果一定要应用这种特性，建议考虑 Blob Storage 或者手动方式计算 File 的 MD5


