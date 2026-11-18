#include <stdio.h>
#include <stdlib.h>
#include <string.h>
typedef struct {
    char hoten[30];
    int masv;
    float d1, d2, d3;
} Data;
typedef struct tagNode {
    Data infor;
    struct tagNode *link;
} Node;
typedef struct {
    Node *pHead;
    Node *pTail;
    int spt;
} LList;
void xoaBoNhoDem() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
}
void initList(LList *L) {
    L->pHead = NULL;
    L->pTail = NULL;
    L->spt = 0;
}
int emptyList(LList L) {
    return (L.spt == 0);
}
Node* createNode(Data x) {
    Node *pp = malloc(sizeof(Node));
    if (pp == NULL) {
        printf("Khong du bo nho!\n");
        return NULL;
    }
    pp->infor = x;
    pp->link = NULL;
    return pp;
}
void insert_Head(LList *L, Data x) {
    Node *pp = createNode(x);
    if (pp == NULL) return;
    if (emptyList(*L)) {
        L->pHead = L->pTail = pp;
    } else {
        pp->link = L->pHead;
        L->pHead = pp;
    }
    L->spt++;
}
void insert_Tail(LList *L, Data x) {
    Node *pp = createNode(x);
    if (pp == NULL) return;
    if (emptyList(*L)) {
        L->pHead = L->pTail = pp;
    } else {
        L->pTail->link = pp;
        L->pTail = pp;
    }
    L->spt++;
}
Node* searchNode(LList L, int maTim) {
    Node *q = L.pHead;
    while (q != NULL && q->infor.masv != maTim) {
        q = q->link;
    }
    return q;
}
void insert_After(LList *L, Node *q, Data x) {
    if (q == NULL) return;
    Node *pp = createNode(x);
    if (pp == NULL) return;
    pp->link = q->link;
    q->link = pp;
    if (q == L->pTail) {
        L->pTail = pp;
    }
    L->spt++;
}
void nhap(Data *x) {
    printf("Nhap ma sv (nhap <= 0 de huy): ");
    scanf("%d", &(x->masv));
    xoaBoNhoDem();
    if (x->masv > 0) {
        printf("Nhap ten sv: ");
        fgets(x->hoten, sizeof(x->hoten), stdin);
        x->hoten[strcspn(x->hoten, "\n")] = '\0';
        printf("Nhap diem 1, 2, 3: ");
        scanf("%f %f %f", &(x->d1), &(x->d2), &(x->d3));
        xoaBoNhoDem();
    }
}
void in(Data x) {
    printf("Ma: %-6d | Ten: %-25s | Diem: %.2f - %.2f - %.2f\n",
           x.masv, x.hoten, x.d1, x.d2, x.d3);
}
void processList(LList L) {
    if (emptyList(L)) {
        printf("Danh sach rong!\n");
        return;
    }
    Node *pp = L.pHead;
    while (pp != NULL) {
        in(pp->infor);
        pp = pp->link;
    }
}
void freeList(LList *L) {
    Node *p = L->pHead;
    while (p != NULL) {
        Node *temp = p;
        p = p->link;
        free(temp);
    }
    initList(L);
}
int main() {
    LList L;
    int chon, maTim;
    Data x;
    initList(&L);
    do {
        printf("\n--- MENU (So phan tu: %d) ---\n", L.spt);
        printf("1: Them dau\n");
        printf("2: Them cuoi\n");
        printf("3: Them sau 1 nut\n");
        printf("9: Duyet ds\n");
        printf("0: Thoat\n");
        printf("Chon: ");
        scanf("%d", &chon);
        xoaBoNhoDem();
        switch (chon) {
            case 1:
                nhap(&x);
                if (x.masv > 0) insert_Head(&L, x);
                break;
            case 2:
                nhap(&x);
                if (x.masv > 0) insert_Tail(&L, x);
                break;
            case 3: {
                printf("Nhap ma sv dung truoc: ");
                scanf("%d", &maTim);
                xoaBoNhoDem();
                Node *q = searchNode(L, maTim);
                if (q == NULL) {
                    printf("Khong tim thay ma sv %d!\n", maTim);
                } else {
                    nhap(&x);
                    if (x.masv > 0) insert_After(&L, q, x);
                }
                break;
            }
            case 9:
                processList(L);
                break;
            case 0:
                freeList(&L); 
                printf("Da giai phong bo nho va thoat!\n");
                break;
            default:
                printf("Lua chon khong hop le!\n");
        }
    } while (chon != 0);
    return 0;
}
