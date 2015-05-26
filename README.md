/* pp-linked-list
   Linked List C++
*/

#include <iostream>
using namespace std;

template <class Item>
class node
{ public:
   typedef Item value_type;

private:
   Item data_field;
   node*link_field;
   
   node(const Item& init_data, node* init_link = NULL)
   { data_field = init_data; link_field = init_link; }

   Item& data()
   { return data_field; }

   node* link()
   { return link_field; }
 
   void set_data(const Item& new_data)
   { data_field = new_data; }

   void set_link(node*new_link)
   { link_field = new_link; }

   //Code to insert one item at the front of the list
   template <class Item>
   void list_head_insert(node<Item>*& head_ptr, const Item& entry)
   { head_ptr = new node<Item>(entry, head_ptr); }
   

   //Code to remove one item from the front of the list
   template <class Item>
   void list_head_remove(node<Item>*&head_ptr)
   { node<Item>*remove_ptr;
     remove_ptr = head_ptr;
	 head_ptr = head_ptr -> link();
	 delete remove_ptr;
   }


   //Code to search for any item in the list
   template <class NodePtr, class Item>
   NodePtr list_search(NodePtr head_ptr, const Item& target)
   { NodePtr cursor;
 
     for(cursor = head_ptr; cursor != NULL; cursor = cursor -> link())
		 if(target == cursor -> data())
			 return cursor;
	 
	 return NULL;
   }
 

   //Code to insert one item anywhere in the list
   template <class Item>
   void list_insert(node<Item> *previous_ptr, const Item &entry)
   { node<Item> *insert_ptr;
     insert_ptr = new node<Item>(entry, previous_ptr -> link());
	 previous_ptr -> set_link(insert_ptr);
   }


   //Code to copy an entire list
   void list_copy(const node<Item>* source_ptr, node<Item>*& head_ptr, node<Item>*& tail_ptr)
   { head_ptr = NULL;
     tail_ptr = NULL;

	 if(source_ptr == NULL)
		 return;
     
	 list_head_insert(head_ptr, source_ptr -> data());
     
	 tail_ptr = head_ptr;   
     source_ptr = source_ptr -> link();
       while(source_ptr != NULL)
	   { list_insert(tail_ptr, source_ptr -> data());
	     tail_ptr = tail_ptr -> link();
		 source_ptr = source_ptr -> link();
       }
   }

 
   //Class where the array is stored
   template <class Item>
   class bag
   { node<Item> *head_ptr;
     size_type many_nodes;

	 //Default constructor
         bag()
	 { head_ptr = NULL;
	   many_nodes = 0;
	 }

	 //Code to insert an item in to the array
         void insert(const Item& entry)
	 { list_head_insert(head_ptr, entry);
	   ++many_nodes;
	 }
	 
	 //Code to eraes one item anywhere in the array
         template <class Item>
	 bool erase_one(const Item& target)
	 { node<Item> *target_ptr;
	   target_ptr = list_search(head_ptr, target);
	   
	   if(target_ptr == NULL)
		   return false;

	   target_ptr -> set_data(head_ptr -> data());
	   list_head_remove(head_ptr);
	   --many_nodes;
	   return true;
	 }


        //Code to insert an item anywhere in to the array
	void insert(const Item& entry)
	{ if(head_ptr == NULL)
   	   { head_ptr = new node<Item>(entry, head_ptr);
     	     tail_ptr = head_ptr;
	     cursor = head_ptr;
	     precursor = NULL;
            }

          else if(is_item() && cursor != head_ptr)
           { precursor = new node<Item>(entry, precursor);
             cursor = precursor -> link();
           }

          else
           { head_ptr = new node<Item>(entry, head_ptr);
             cursor = head_ptr;
             precursor = NULL;
           }

         many_nodes++;

        }


       //Code to remove an item from the array where the cursor is
       void remove_current()
       { assert(is_item());
  
         if(cursor == head_ptr)
           head_ptr = head_ptr -> link();
  
         if(cursor == tail_ptr)
	   tail_ptr = precursor;
  
         cursor = cursor -> link();

         if(precursor != NULL)
	   precursor -> set_link(cursor);

         many_nodes--;
        
         }


       //Code to overload the += operator to add two linked lists together
       void operator+=(const sequence<Item>& addend)
       { node<Item>* copy_head_ptr;
         node<Item>* copy_tail_ptr;
   
          if(addend.many_nodes > 0)
           { list_copy(addend.head_ptr, copy_head_ptr, copy_tail_ptr);
             copy_tail_ptr -> set_link(head_ptr);
	     head_ptr = copy_head_ptr;
	     many_nodes += addend.many_nodes;
           }
        }


      //Code to overload the + operator to add two linked lists together
      friend sequence<Item> operator+(sequence<Item> s1, sequence<Item> s2)
      { sequence<Item> answer;
        answer += s1;
        answer += s2;
        return answer;
       }



