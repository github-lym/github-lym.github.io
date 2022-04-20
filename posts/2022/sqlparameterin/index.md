# 在語法有IN情況下使用Sql Parameter


之前為了資安問題都用**Sql Parameter**。    
<!--more-->

但後來遇到參數個數不定，須用`in`的情況。  
好在有找到答案。  
```CSharp
if (TeachingObject != null && TeachingObject.Length > 0)
{
	var parameters = new string[TeachingObject.Length];
	for (int i = 0; i < TeachingObject.Length; i++)
	{
		parameters[i] = string.Format("@TeachingObject{0}", i);
		paras.Add(new SqlParameter(parameters[i], TeachingObject[i]));
	}
	sqlCmd += string.Format(" AND A.TeachingObject IN ({0})", string.Join(", ", parameters));
}
```

