#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>
#include <malloc.h>
#include <conio.h>
#include <windows.h>
struct thongtinsv{
	char hoten[20];
	char namsinh[25];
	double diemtb;
};
typedef struct thongtinsv sv;
struct node{
	sv data;
	struct node *next;
};
typedef struct node node;
sv nhapthongtinsv(){
	sv x;
	printf("\tNhap ho ten sinh vien:");
	fflush(stdin);
	gets(x.hoten);
	printf("\tNhap nam sinh cua sinh vien:");
	fflush(stdin);
	gets(x.namsinh);
	printf("\tNhap diem trung binh cua sinh vien:");
	scanf("%lf", &x.diemtb);
	return x;
}
node *makenode(sv x){
	node *newnode = (node *)malloc(sizeof(node));
	newnode->data = x;
	newnode->next = NULL;
	return newnode;
}
void pushback(node **head, sv x){
	node *newnode = makenode(x);
	node *tmp = *(head);
	if(tmp == NULL){
		*(head) = newnode;
		return;
	}
	while(tmp->next != NULL){
		tmp = tmp->next;
	}
	tmp->next = newnode;
}
void pushfront(node **head, sv x){
	node *newnode = makenode(x);
	newnode->data = x;
	newnode->next = (*head);
	(*head) = newnode;
}
void taodanhsach(node **head){
	printf("Vui long nhap so luong sinh vien can tao: ");
	int n;
	scanf("%d", &n);
	for(int i = 1; i <= n; i++){
		printf("Nhap sinh vien thu %d\n", i);
		sv x = nhapthongtinsv();
		pushback(head, x);
	}
	return;
}
void duyetdanhsach(node *head){
	if(head == NULL){
		printf("Khong co sinh vien nao ca !\n");
		return;
	}
	printf("\tDanh sach toan bo sinh vien la:\n");
	while(head != NULL){
		printf("%5s %5s %5lf\n", head->data.hoten, head->data.namsinh, head->data.diemtb);
		head = head->next;
	}
}
void erase(node **head){
	char c[20];
	node *prev = NULL;
	node *cur = *head;
	if(cur == NULL){
		printf("EMPTY\n");
		return;
	}
	printf("Vui long nhap ten can xoa la:");
	getchar();
	fgets(c, 20, stdin);
	c[strcspn(c, "\n")] = '\0';
	if(cur == NULL){
		return;
	}
	while(cur != NULL){
		if(strcmp(cur->data.hoten, c) == 0){
			if(cur == *head){
				*head = cur->next;
			}
			 else {
				prev->next = cur->next;
			}
			free(cur);
			return;
		}
		prev = cur;
		cur = cur->next;
	}
	printf("Khong tim thay sinh vien can xoa!\n");
}
int size(node *head){
	int cnt = 0;
	while(head != NULL){
		++cnt;
		head = head->next;
	}
	return cnt;
}
void popfront(node **head){
	if(*head == NULL){
		printf("Deo co ai o dau de xoa!\n");
		return;
	}
	else{
		node *tmp = (*head);
		*head = (*head)->next;
		free(tmp);
	}
}
void insert(node **head){
	printf("Nhap thong tin sinh vien can chen: \n");
	sv x = nhapthongtinsv();
	printf("Nhap vi tri chen:\n");
	int k;
	scanf("%d", &k);
	int n = size(*head);
	if(k < 1 && k > n + 1){
		printf("vi tri chen khong hop le !\n");
		return;
	}
	else{
		if(k == 1){
			pushfront(head, x);
		}
		else if(k == n + 1){
			pushback(head, x);
		}
		else{
			node *tmp = *(head);
			for(int i = 1; i < k - 1; i++){
				tmp = tmp->next;
			}
			node *newnode = makenode(x);
			//buoc 1
			newnode->next = tmp->next;
			//buoc 2
			tmp->next = newnode;
		}
	}
}
void popback(node **head){
	//xoa  cuoi
	if(*head == NULL){
		printf("Deo co ai o cuoi de xoa!\n");
		return;
	}
	else{
	node *tmp = *head;
	if(tmp->next == NULL){
		*head = NULL;
		free(tmp);
		return;
	}
	while(tmp->next->next != NULL){
		tmp = tmp->next;
	}
	node *last = tmp->next;
	tmp->next = NULL;
	free(last);
	}
}
void eraseall(node **head){
    char c[20];
    node *prev = NULL;
    node *cur = *head;
    if(cur == NULL){
        printf("EMPTY\n");
        return;
    }
    printf("Vui long nhap ten can xoa la:");
    getchar();
    fgets(c, 20, stdin);
    c[strcspn(c, "\n")] = '\0';
    while(cur != NULL && strcmp(cur->data.hoten, c) == 0){
        node *tmp = *head;
        cur = cur->next;
        free(tmp);
    } 
}
void del(node **head){
	if(*head == NULL){
		printf("Danh sach sinh vien rong nen khong the xoa!\n");
		return;
	}
	printf("Nhap vi tri xoa\n");
	int k;
	scanf("%d", &k);
	if(k < 1 || k > size(*head)){
		return;
	}
	if(k == 1){
		popfront(head);
	}
	else{
		node *tmp = (*head);
		for(int i = 1; i < k - 1; i++){
			tmp = tmp->next;
		}
		node *del = tmp->next;
		tmp->next = del->next;
		free(del);
	}
}
void delall(node **head) {
    node *cur = *head;
    if(cur == NULL){
    	printf("Khong co ai ma bat tao xoa ?\n");
    	return;
	}
    while (cur != NULL) {
        *head = cur->next;
        free(cur);
        cur = *head;
    }
    printf("Danh sach sinh vien da duoc xoa het!\n");
}
void sort(node **head){
	if(*head == NULL){
		printf("Khong co sinh vien de sap xep!\n");
		return;
	}
	for(node *i = (*head); i != NULL; i = i->next){
		node *min = i;
		for(node *j = i->next; j != NULL; j = j->next){
			if(min->data.diemtb > j->data.diemtb){
				min = j;
			}
		}
		sv tmp = min->data;
		min->data = i->data;
		i->data = tmp;
	}
}
void MENU(){
		printf("\t\t\t\t\t\t\t------\n");
		printf("\t\t\t\t\t\t\t|MENU|\n");
		printf("\t\t\t\t\t\t\t------\n\n");
		printf("\t _______________________________________________________________________________________________\n");
    	printf("\t| 1. nhap mot danh sach sinh vien                   | 8. Xoa mot sinh vien o cuoi danh sach     |\n");
   		printf("\t| 2. Hien thi danh sach sinh vien                   | 9. Them mot sinh vien vao dau danh sach   |\n");
   		printf("\t| 3. So sinh vien trong danh sach                   | 10. Xoa mot sinh vien o dau danh sach     |\n");
  		printf("\t| 4. Xoa toan bo sinh vien co ten theo ten ban nhap | 11. Them mot sinh vien vao vi tri k       |\n");
    	printf("\t| 5. Xoa mot lan mot sinh vien theo ten             | 12. Xoa mot sinh vien o vi tri k          |\n");
    	printf("\t| 6. Sap xep danh sach sinh vien theo diem tang dan | 13. Xoa toan bo sinh vien                 |\n");
    	printf("\t| 7. Them mot sinh vien vao cuoi danh sach          | 14.                                       |\n");
    	printf("\t| 8. Xoa mot sinh vien o cuoi danh sach             | 15.                                       |\n");
    	printf("\t| 9. Them mot sinh vien vao dau danh sach           | 0. Exit                                   |\n");
    	printf("\t|___________________________________________________|_________________________________________ _|\n");
}
int main(){
	node *head = NULL;
	while(1){
		MENU();
    	printf("\t\tVui long chon: ");
    	int lc;
    	scanf("%d", &lc);
    	if(lc == 0){
    		printf("Ket thuc chuong trinh!\n");
    		return 0;
		}
		else if(lc == 1){
			taodanhsach(&head);
		}
		else if(lc == 2){
			duyetdanhsach(head);
		}
		else if(lc == 7){
			sv x = nhapthongtinsv();
			pushback(&head, x);
		}
		else if(lc == 9){
			sv x = nhapthongtinsv();
			pushfront(&head, x);
		}
		else if(lc == 5){
			erase(&head);
		}
		else if(lc == 4){
			eraseall(&head);
		}
		else if(lc == 3){
			if(head == NULL){
				printf("Khong co sinh vien nao ca!\n");
				return 0;
			}
			printf("So sinh vien trong danh sach la : %d", size(head));
		}
		else if(lc == 8){
			popback(&head);
		}
		else if(lc == 10){
			popfront(&head);
		}
		else if(lc == 11){
			insert(&head);
		}
		else if(lc == 12){
			del(&head);
		}
		else if(lc == 6){
			sort(&head);
		}
		else if(lc == 13){
			delall(&head);
		}
		else{
			printf("Ban da nhap sai vui long an(y) de quay ve MENU!\nNeu khong muon hay an phim bat ky de ket thuc chuong trinh!\n");
			char c[256];
			getchar();
			scanf("%c", c);
			if(c == "y"){
				MENU();
			}
			else{
				printf("Ket thuc chuong trinh!\n");
    		return 0;
			}	
		}
	}
}
