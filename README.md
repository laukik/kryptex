Kryptex version 3.1
==============================================================================

To install all dependencies first install node.js, then run
	
	npm install -d

===============================================================================

"redis schema":
	we have following detail to store,

		1) users
		---------------------------------------------------------------------------------------
		  name = user:[id*]  (*here id is userid generated from md5, giving 128 bit hash of email address) 
		  type = hashset
		---------------------------------------------------------------------------------------
		feild_names :

			1. name
			2. email
			3. ph_no
			4. current_level
			5. tols                ( time of last correct submit )
			6. ts                  ( total number of answer submitted both correct and wrong just for tie-breaker)

		2) level_count
		---------------------------------------------------------------------------------------
		  name = level_count
		  type = string
		  value = number of levels resides in database
		---------------------------------------------------------------------------------------
		
		3) level
		---------------------------------------------------------------------------------------
		  name = level:[id]  (id = 0 to level_count)
		  type = list
		---------------------------------------------------------------------------------------
		feild_names :
		
		  	list[0] = Question path
		  	list[1] = SOLUTION
		 ---------------------------------------------------------------------------------------

		4) Result
		---------------------------------------------------------------------------------------
		  name = result
		  type = Sortedset
		---------------------------------------------------------------------------------------
		feild_names :
			score will be current level i.e current_level of user:id
			value = tols of user:id  + userid of user:id
			so overall value will be:
					example:

					score			value
					=================================================
					0    			1123332:23sad34234cad344
					0				1223434:s4sed342u4c3d37u
					1 				2345545:ter6723ksjdfkh2y
					|					|		|
					|--> current level  |		|-----> md5 generated hash also used in user:id
										|
										|
										|--> last correct solution timestamp
